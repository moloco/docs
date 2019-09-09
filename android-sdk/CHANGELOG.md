# Moloco Android SDK - Changelog

## Version 1.1.5 (Sep 09, 2019)
* Receive `FinalLandingUrl` from Adserver.
* Add `landingUrl` to the arguments of `handleResolvedUrl` on `UrlHandler`.
  * You should reimplement `UrlHandler` if you want to apply this version.

## Version 1.1.4 (August 30, 2019)
* Fill the device info on the ad request. This enables to send the device info to Moloco ad server.

## Version 1.1.3 (August 21, 2019)
* Add an interface `UrlHandler` for client.

## Version 1.1.2 (July 15, 2019)
* Enable to set keywords on a MolocoView.

## Version 1.1.1 (June 03, 2019)
* Make JAVA 7 compatible for a whole SDK project.
