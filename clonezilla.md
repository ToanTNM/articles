# Clonezilla guide

## Prepare

1. Download clonezilla live (iso or zip file) from https://clonezilla.org/downloads.php or https://free.nchc.org.tw/clonezilla-live

2. Following the [instruction](https://clonezilla.org/clonezilla-live.php) to create USB boot

## Clone local disk to remote computer disk

1. Boot from usb, choose 1st option

   ![Step 1](/images/clonezilla/step1.png)

1. Choose default or what you want in next 2 step

   ![Step 2.1](/images/clonezilla/step2.1.png)

   ![Step 2.2](/images/clonezilla/step2.2.png)

1. Start clonezilla

   - Choose `Start clonezilla` to begin

     ![Step 3.1](/images/clonezilla/step3.1.png)

   - Choose `lite-server` to clone from local source to remote

     ![Step 3.2](/images/clonezilla/step3.2.png)

   - Choose `lite-server` -> `start` to clone from local source to remote

     ![Step 3.2](/images/clonezilla/step3.2.png)

     ![Step 3.3](/images/clonezilla/step3.3.png)

   - Choose `netboot`

     ![Step 3.4](/images/clonezilla/step3.4.png)

     choose `use-existing-dhcp` > `dhcp` if these is existing dhcp server in network.

     choose `start-new-dhcpd` if there is no dhcp server, clonezilla will do itself

     ![Step 3.5](/images/clonezilla/step3.5.png)

     ![Step 3.6](/images/clonezilla/step3.6.png)

   - choose `localdev` if you want to read image file to local disk.

     choose `skip` if not (we choose `skip` now because we will clone from disk to disk later)

     ![Step 3.7](/images/clonezilla/step3.7.png)

   - Choose `Expert` > `massive-deployment`

     ![Step 3.8](/images/clonezilla/step3.8.png)

     ![Step 3.9](/images/clonezilla/step3.9.png)

   - Choose `from-image` if previous step is `localdev`

     Choose `from-device` > `disk-2-mdisks`

     ![Step 3.10](/images/clonezilla/step3.10.png)

     ![Step 3.11](/images/clonezilla/step3.11.png)

   - Choose **source** disk to clone to remote computer

     ![Step 3.12](/images/clonezilla/step3.12.png)

   - Choose options before start

     ![Step 3.13](/images/clonezilla/step3.13.png)

     ![Step 3.14](/images/clonezilla/step3.14.png)

   - Choose what to do when done

     ![Step 3.15](/images/clonezilla/step3.15.png)

   - Choose `multicast` > `clients+time-to-wait`

     ![Step 3.16](/images/clonezilla/step3.16.png)

     ![Step 3.17](/images/clonezilla/step3.17.png)

   - Enter `clients` (number of clients will be clone)

     ![Step 3.18](/images/clonezilla/step3.18.png)

   - Enter `time-to-wait` (waiting time befor start, even if clients is smaller than value in previous step)

     ![Step 3.19](/images/clonezilla/step3.19.png)

   - Clonezilla will be run as server

     ![Step 3.20](/images/clonezilla/step3.20.png)

1. In remote computers

   - Boot from network (in the same network with clonezilla server)

     ![Step 4.1](/images/clonezilla/step4.1.png)

   - All action will be automatically run, when done, the computers will do the action we choose before (`reboot`)

     ![Step 4.2](/images/clonezilla/step4.2.png)

     ![Step 4.3](/images/clonezilla/step4.3.png)

1. Shutdown clonezilla server by enter `y`

   ![Step 5](/images/clonezilla/step5.png)

## Create clonezilla live

1. After language and keyboard are selected, choose **Start_Clonezilla** -> **device-image**, then mount a working directory, the space should large enough to put the live CD and some temp files. It's recommended to choose local*dev to mount local partition as */home/partimag\_

   When Clonezilla live asks you to choose save or restore disk/partition, choose **exit** to enter command line prompt

1. Run these command

   ```js
   sudo -i

   // configure the network to update package
   ocs-live-netcfg

   apt-get purge drbl clonezilla
   apt-get update

   apt-get -y install drbl clonezilla

   apt-get -y install live-build

   cd /home/partimg

   // create debian iso template to create clonezilla in next step
   create-debian-live -d sid -i mt

   // create iso file
   ocs-iso -s -x quiet -j debian-live-for-ocs-mt.iso -i mt -k NONE -g en_US.UTF-8

   // create zip file
   ocs-live-dev -s -x quiet -j debian-live-for-ocs-mt.iso -i mt -k NONE -g en_US.UTF-8
   ```

Referrences:

- https://clonezilla.org/create_clonezilla_live_from_scratch.php

- https://github.com/stevenshiau/clonezilla/blob/master/sbin/create-debian-live

- https://github.com/stevenshiau/clonezilla/blob/master/sbin/ocs-iso

## ssh to clonezilla

1. Run step 1 in [Create clonezilla live](#create-clonezilla-live)

1. Start ssh service in clonezilla

   ```js
   sudo -i
   // configure the network
   ocs-live-netcfg

   // start service
   service ssh start
   service ssh status
   ```

1. Connect from client

   ```js
   // connect to clonezilla and enter password 'live'
   ssh user@[ip of clonezilla]

   // after connect run command
   sudo -i
   ...
   ```

Referrences:

- https://clonezilla.org/create_clonezilla_live_from_scratch.php

- https://nhattruong.blog/2021/07/12/ssh-vao-may-chu-ubuntu-virtualbox-cau-hinh-nat/
