---
title: How to restore access to a VM in {{ compute-full-name }}
description: Follow this guide to restore access to a VM.
---

# Restoring access to a VM

You may need to restore access to a [VM](../../concepts/vm.md) in the following cases:
* [VM user credentials are lost](#cloud-init)
* [The public part of an SSH key was changed](#ssh-recovery)
* [The SSH connection fails](#serial-console)
* [The VM fails to start](#os-recovery)

## VM user credentials are lost {#cloud-init}

If you lost your private SSH key for Linux or your Windows user password:
1. [Create a snapshot](../disk-control/create-snapshot.md) of the [disk](../../concepts/disk.md) of the VM that you want to restore access to.
1. If you find it important to keep the [public IP address](../../../vpc/concepts/address.md#public-addresses) used by the current VM, [make](../../../vpc/operations/set-static-ip.md) this IP address static.
1. [Stop](../vm-control/vm-stop-and-start.md#stop) the VM and [unlink](../vm-control/vm-detach-public-ip.md) the public IP address from it.
1. [Create a new VM with the disk from the snapshot](../vm-create/create-from-snapshots.md) by specifying this disk as the boot disk. When creating a VM, enter your new credentials under **{{ ui-key.yacloud.compute.instances.create.section_access }}**.

    If you had previously made the VM's public IP address static, use that IP address as the public IP address of the new VM:

    {% list tabs group=instructions %}

    - Management console {#console}

      In the **{{ ui-key.yacloud.component.compute.network-select.field_external }}** field, select the `{{ ui-key.yacloud.component.compute.network-select.switch_list }}` address assignment method and select the previously reserved IP from the list that opens.

    - CLI {#cli}

      In the `yc compute instance create` command, provide the previously reserved IP address in the `nat-address` field of the `--network-interface` parameter. Here is an example:
      
      ```bash
      yc compute instance create \
        ...
        --network-interface subnet-name=subnet-b,nat-ip-version=ipv4,nat-address=111.112.13.14 \
        ...
      ```

      For more information about the `yc compute instance create` command, see the [CLI reference](../../../cli/cli-ref/compute/cli-ref/instance/create.md).

    - {{ TF }} {#tf}

      In the `network_interface` section of the `yandex_compute_disk` resource, specify the previously reserved IP address in the `nat_ip_address` parameter. Here is an example:

      ```hcl
      network_interface {
        subnet_id      = "e2lqsms4cdl3********"
        nat            = true
        nat_ip_address = "111.112.13.14"
      }
      ```

      For more information about the `yandex_compute_disk` resource, see the [relevant provider documentation]({{ tf-provider-resources-link }}/compute_instance).

    {% endlist %}

1. Connect to the VM via [SSH](./ssh.md) or [RDP](./rdp.md) using the new credentials.
1. Once you know that the new VM is up and running, [delete](../vm-control/vm-delete.md) the old VM and the disk [snapshot](../snapshot-control/delete.md).

    If you do not delete them, they will remain billable, and {{ yandex-cloud }} will continue to charge for them.

If the 'cloud-init' or [network](../../../vpc/concepts/network.md#network) configuration was changed for the VM, the described method may not work. In this case, see [the VM fails to start](#os-recovery).

## The public part of an SSH key was changed {#ssh-recovery}

You may have problems accessing a Linux-based VM over SSH if the public part of your SSH key was changed or deleted on the side of the VM.

You can restore access to a Linux-based VM over SSH using the [serial console](#serial-console) if you have a valid pair of SSH keys.

If you can't access the serial console, do the following to recover the public part of your SSH key:
1. [Stop the VM](../vm-control/vm-stop-and-start.md).
1. [Take a snaphost](../disk-control/create-snapshot.md) of the VM's boot disk.
1. [Create](../vm-create/create-from-snapshots.md) an auxiliary Linux-based VM. Under **{{ ui-key.yacloud.compute.instances.create.section_image }}**, choose the OS for the boot disk. To attach a data disk to an auxiliary VM, select the previously created snapshot in the **{{ ui-key.yacloud.compute.instances.create.section_storages }}** section.
1. [Connect over SSH](../vm-connect/ssh.md) to the auxiliary VM and [mount the disk](../vm-control/vm-attach-disk.md#mount-disk-and-fix-uuid) you created from the snapshot.
1. Replace the SSH key that is stored on this disk with a valid key:
   1. Go to the [folder](../../../resource-manager/concepts/resources-hierarchy.md#folder) that stores the public part of the SSH key in the mounted partition. For example:

      ```bash
      cd /mnt/home/<username>/.ssh
      ```

      By default, user SSH keys are stored in the user's `~/.ssh` folder.
   1. Open the `authorized_keys` file, e.g., using the `nano` text editor:

      ```bash
      nano authorized_keys
      ```

      If the file is missing, this command will create it.
   1. Replace the contents of the file with the public part of the valid SSH key pair.
1. Unmount the disk with the fixed SSH key from the VM:

   ```bash
   sudo umount /mnt
   ```

1. [Stop](../vm-control/vm-stop-and-start.md) the auxiliary VM and [detach](../vm-control/vm-detach-disk.md) the disk with the fixed SSH key from it.
1. [Create](../vm-create/create-from-disks.md) a new VM. Under **{{ ui-key.yacloud.compute.instances.create.section_image }}**, go to the **{{ ui-key.yacloud.compute.instances.create.image_value_custom_new }}** tab and select the disk with the fixed SSH key as the boot disk.

   {% note info %}

   Make sure the new VM and the disk with the fixed SSH key are in the same [availability zone](../../../overview/concepts/geo-scope.md).

   {% endnote %}

1. [Connect](../vm-connect/ssh.md) to the new VM over SSH.
1. [Delete the disk snapshot](../snapshot-control/delete.md) and [delete](../vm-control/vm-delete.md) the auxiliary and old VMs.

    If you do not delete them, they will remain billable, and {{ yandex-cloud }} will continue to charge for them.

## The SSH connection fails {#serial-console}

The problem may occur due to an error in the SSH, [security group](../../../vpc/concepts/security-groups.md), or network settings. To restore access, connect to the VM using the [serial console](../serial-console/index.md) and adjust the settings.

{% list tabs group=operating_system %}

- Linux {#linux}

  {% note info %}

  You may use the serial console only if the user password is set; otherwise, see [The VM fails to start](#os-recovery).

  {% endnote %}

  1. [Enable access](../serial-console/index.md#turn-on-for-current-instance) to the VM serial console.
  1. [Connect](../serial-console/connect-cli.md#connect-to-serial-console) to the VM serial console.
  1. Change the SSH or network settings. If you have security groups set up, make sure that their rules allow incoming TCP traffic to port 22.
  1. Connect to the VM via SSH.


{% endlist %}

## An SSH key is lost {#lost-ssh}

{% include [new-connect-ssh](../../../_qa/compute/new-connect-ssh.md) %}

## The VM fails to start {#os-recovery}

If you cannot start a VM, get access to data on the disk as follows:
1. [Create a snapshot](../disk-control/create-snapshot.md) of the disk of the VM that you want to restore access to.
1. [Create a disk](../disk-create/empty.md) from the snapshot. When creating a disk, choose `{{ ui-key.yacloud.compute.instances.create-disk.value_source-snapshot }}` in the **{{ ui-key.yacloud.compute.instances.create-disk.field_source }}** field and specify the created snapshot.
1. [Attach the disk](../vm-control/vm-attach-disk.md) as secondary to the auxiliary VM.
1. Change the configuration files that affect the VM startup or copy important data.
1. [Detach the disk](../vm-control/vm-detach-disk.md) from the auxiliary VM.
1. If you found and fixed the startup issue, create a new VM. When creating a VM, go to the **{{ ui-key.yacloud.compute.instances.create.image_value_custom_new }}** tab under **{{ ui-key.yacloud.compute.instances.create.section_image }}** and select the fixed disk as the boot disk.
1. After restoring access to the VM, do not forget to delete unused resources: [VMs](../vm-control/vm-delete.md), [disks](../disk-control/delete.md), and [disk snapshots](../snapshot-control/delete.md). If you do not delete them, they will remain billable, and {{ yandex-cloud }} will continue to charge for them.