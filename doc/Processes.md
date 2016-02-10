# Xcode Processes

The following processes need to be automated in order to perfect your dev team
build cycles.

## Distributing to Testers

This can happen however you like as long as you have the following...

- A packaged app -- .ipa file
- Provisioning Profile

## Installing Certificates

Create a keychain for (jenkins) automation.

    security unlock -p jenkins jenkins

    security import provisioning/ios_distribution.p12 \
             -k ~/Library/Keychains/jenkins \
             -P jenkins

## IPA Building & Code Signing

## OTA Install


