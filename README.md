# xcode-vm #

This project was started in order to automate the builds of iOS and OS X apps
natively using Xcode but within a portable development VM.

Running OS X inside a VM is challenging. This project presents a start.

Currently, the VM includes...

* Your licensed copy of OS X as a Vagrant base box.
* OS X setup for headless development support.

Things left to script...

* Scripted installation of Xcode and any other Apple dev dependency.
* Included [Fastlane](https://github.com/fastlane/fastlane) for build workflows.
* Included [Gym](https://github.com/fastlane/gym) for building and codesigning iOS apps.
* Installation of [Homebrew](http://brew.sh) and [Cocoapods](https://cocoapods.org) for package management.
* Utilization for processing CD/CI jobs.

You'll need to use your personal, licensed copy of OS X in order to build this
VM. Everything else will be automatically installed for you (or will be in
time).

This setup works with Vagrant since [boxcutter/osx][1] provides a well supported
process and Packer template. Hopefully in the future we'll get the time to test
running this VM inside __other__ environments.

Officially, you can only run Apple's Mac OS X on Apple products. Anything else
would make the ghost of Steve Jobs very sad.

[1]: https://github.com/boxcutter/osx

## Requirements ##

* Homebrew
* w/ Brewfile support `brew tap homebrew/bundle`

## Setup ##

### Clone this Project

Start by cloning this project, note the use of `--recursive` for cloning git
submodules as well.

    $ git clone --recursive https://github.com/cheapRoc/xcode-vm
    $ cd xcode-vm

### Install Homebrew

Install our local dependencies (because you have Homebrew preinstalled).

    $ brew tap homebrew/bundle
    $ brew bundle

### Download OS X

Make sure you've downloaded a copy of the OS X v10.11 El Capitan installation
media (huge app containing the OS X disk image).

- Download OS X from the App Store
- It'll end up in `/Applications/Install OS X El Capitan.app`

### Clone boxcutter/osx

Unless you have an `./osx` directory with files, clone `boxcutter/osx` off of
GitHub. This performs all of the work in turning your OS X installation media
into a proper Vagrant base box.

    $ git clone https://github.com/boxcutter/osx
    $ cd osx

### Prepare the OS X Media

From the instructions in `boxcutter/osx` we'll need to prepare the DMG as a
patched ISO.

This script performs a bunch of helpful things like removing all of the OS X
installation steps, configuring post-install hooks, and setting up accounts.

    $ sudo ./prepare_iso/prepare_iso.sh -D DISABLE_REMOTE_MANAGEMENT /Applications/Install\ OS\ X\ El\ Capitan.app dmg

### Pack OS X into a Box

Next, we'll use Packer to automate the generation of our Vagrant base image/box.

    $ packer build -only=virtualbox-iso -var-file=osx1011.json osx.json

### Add OS X to Vagrant

Now we'll add our newly minted base box into Vagrant. Take note of the name
we've given the box, `osx1011`.

    $ vagrant box add ./box/virtualbox/osx1011-nocm-0.1.0.box --name osx1011

### Configure a Vagrantfile

Next, create your own `Vagrantfile` via `vagrant init`, or copy our
`Vagrantfile` as your template.

Vagrant machines running OS X will need extra resources in order to run. You'll
probably want to mess around with VirtualBox settings until you've found the
right set of resources for your virtualization host environment.

### Vagrant Up

Now `vagrant up` and `vagrant ssh` in order to log in.

If things worked out and you've SSH'd into your OS X VM, you can `vagrant halt`
for now.

### Download Xcode

Next we'll download Xcode.

Like OS X, you'll need to manually download Xcode from Apple.

Unlike OS X, we won't need the App Store if you've got an Apple Developer
account.

Click the following link to download the installation disk image. You'll be
prompted to log into your Apple Developer account first.

- [Download Xcode 7.2.1](https://developer.apple.com/services-account/download?path=/Developer_Tools/Xcode_7.2.1/Xcode_7.2.1.dmg)

### Copy & Mount Xcode

Copy the downloaded file into our work directory and mount it into the VM's file system.

     $ vagrant halt
     $ cp ~/Downloads/Xcode_7.2.1.dmg ./
     $ vagrant up
     $ vagrant ssh
     $ hdiutil attach -nobrowse /vagrant/Xcode_7.2.1.dmg
     $ ls -la /Volumes/Xcode

If everything worked, you should have listed the newly mounted directory
`/Volumes/Xcode`. This includes the Xcode installation files.

### Install Xcode

Next, we'll perform a headless install of Xcode and Xcode Server.

**WARNING:** This ain't done...

## ...but we're close!

So far, I've proven out that you can at least install Xcode Server and run it's
wrapped services (CouchDB, Nginx, Redis).
