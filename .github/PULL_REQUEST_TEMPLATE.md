<!--- Provide a general summary of your changes in the Title above -->
<!--- If you're unsure about anything in this checklist, don't hesitate to create a PR and ask. I'm happy to help! -->

# Description

<!---
Please name items in `BRAND TYPE (Model XYZZY)` format to make the sorts consistent and useful - it's nicer to see all the smart buttons of a given brand sorted together.

Please format Zigbee, Z-Wave and WIFI entries as
```
| Type | Brand | Model-link | Notes |
```
--->

This way all the switches will sort together, sensors together, etc.

<!---
Notes suggestions:

- If you're adding an entry for something that _doesn't_ work, those have been moved to https://github.com/unixorn/internet-of-trash
- If the device you're adding in your PR won't work in a local-only mode, please add it to https://github.com/unixorn/internet-of-trash
- If it requires a remote service, definitely submit it to https://github.com/unixorn/internet-of-trash - I prefer to not buy anything that will brick if the vendor goes out of business or decides to cancel the product line, or even if your internet is just having an outage.
- If it tries to phone home - note that too.
- If you have to reflash a device to get it working with Home Assistant, please add that to the **Notes** column. Ideally add a link to the reflash instructions for the device
- If you need to add a plugin to Home Assistant before it can be used, add that to **Notes** too.
- If it requires the devices connect to an internet server, even just for initial configuration, please add that to **Notes** - I want it easy to see which devices won't brick if the vendor goes out of business and also aren't vulnerable to some jackass hacking the company's servers.
-->
# Type of changes

<!--- What types of changes does your submission introduce? Put an `x` in all the boxes that apply: -->

- [ ] A link to an external resource like a blog post, video or tutorial
- [ ] Add/remove/update a device entry
- [ ] Text cleanups/typo fixes

# Copyright Assignment

- [ ] This document is covered by the [Apache License](https://github.com/unixorn/works-with-home-assistant/blob/master/LICENSE), and I agree to contribute this PR under the terms of the license.

# Checklist:

<!---
Go over all the following points, and put an `x` in all the boxes that apply. Make them look like [x] so that GitHub's markdown renderer renders them properly.
-->

- [ ] **I have personally used the device(s), tools or utilities being added.**
- [ ] **If this device, tool or utility requires an internet server to operate or be configured, it is noted in the entry.** Warn people that it could brick if the company shuts down like Insteon did.
- [ ] I have read the [Contributing](https://github.com/unixorn/works-with-home-assistant/blob/master/Contributing.md) document.
- [ ] I have signed off my commits. You can use `git commit --amend --no-edit --signoff` to amend an existing commit, and you can find more details about signing off your commits on the DCO GitHub action page [here](https://probot.github.io/apps/dco/)
- [ ] All new and existing tests passed.
- [ ] I have confirmed that the link(s) in my PR are valid.
- [ ] Any links to any online vendors do _not_ include referral codes or embedded tracking codes.
- [ ] Entries are single lines and are in the appropriate (Hub, Zigbee, Z-Wave, Tools or WIFI) section, and are added in alphabetical order in their section.
