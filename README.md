# xcode-vm

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

## Requirements

* Homebrew
* w/ Brewfile support `brew tap homebrew/bundle`

## Setup

Start by cloning this project, note the use of `--recursive` for cloning git
submodules as well.

    $ git clone --recursive https://github.com/cheapRoc/xcode-vm
    $ cd xcode-vm

Install our local dependencies (because you have Homebrew preinstalled).

    $ brew tap homebrew/bundle
    $ brew bundle

Make sure you've downloaded a copy of the OS X v10.11 El Capitan installation
media (huge app containing the OS X disk image).

- Download OS X from the App Store
- It'll end up in `/Applications/Install OS X El Capitan.app`

Next, we'll clone `boxcutter/osx` off of GitHub in order to help us turn our OS
X installation media into a Vagrant base box.

    $ git clone https://github.com/boxcutter/osx
    $ cd osx

From the instructions in `boxcutter/osx` we'll need to prepare the DMG as a
patched ISO.

    $ sudo ./prepare_iso/prepare_iso.sh -D DISABLE_REMOTE_MANAGEMENT /Applications/Install\ OS\ X\ El\ Capitan.app dmg

Next, we'll use Packer to automate the generation of our Vagrant base image/box.

    $ packer build -only=virtualbox-iso -var-file=osx1011.json osx.json

Then add this base box to Vagrant, taking note of the name of our base
`osx1011`.

    $ vagrant box add ./box/virtualbox/osx1011-nocm-0.1.0.box --name osx1011

Next, create your own `Vagrantfile` via `vagrant init`, or copy our
`Vagrantfile` as your template.

Vagrant machines running OS X will need extra resources in order to run. You'll
probably want to mess around with VirtualBox settings until you've found the
right set of resources for your virtualization host environment.

**WARNING:** This ain't done.
