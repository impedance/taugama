ZeroOmega, forked from SwitchyOmega compatible with manifest v3
============

[Chrome Web Store](https://chromewebstore.google.com/detail/pfnededegaaopdmhkdmcofjmoldfiped)

[Microsoft Edge Addons](https://microsoftedge.microsoft.com/addons/detail/zeroomegaproxy-switchy-/dmaldhchmoafliphkijbfhaomcgglmgd)

[Firefox Addon](https://addons.mozilla.org/en-US/firefox/addon/zeroomega/)

Manage and switch between multiple proxies quickly & easily.

[![Translation status](https://hosted.weblate.org/widgets/switchyomega/-/svg-badge.svg)](https://hosted.weblate.org/engage/switchyomega/?utm_source=widget)

<!-- AICODE-NOTE: NAV/README-INDEX - README is the primary repository map for agents; ref: AGENTS.md, docs/aicode.md; risk: hard-to-find entry points during changes -->

Agent navigation (AICODE)
-------------------------

This section is a lightweight index for contributors/agents. It follows `AGENTS.md` and `docs/aicode.md`.

### Repository layout

- `AGENTS.md` - agent protocol (what to read/run first); search: `rg -n "AICODE-"`.
- `docs/aicode.md` - AICODE anchor rules/schema; search: `rg -n "AICODE-" docs/aicode.md`.
- `omega-build/` - build orchestrator (grunt hub + cross-module scripts); entry: `omega-build/package.json`.
- `omega-pac/` - PAC generator module (profiles -> PAC script); entry: `omega-pac/index.coffee`; search: `rg -n "PacGenerator" omega-pac/src`.
- `omega-target/` - browser-independent options manager & core logic; entry: `omega-target/index.coffee`; search: `rg -n "class Options" omega-target/src/options.coffee`.
- `omega-web/` - web-based configuration UI (Angular); entry: `omega-web/src/`; search: `rg -n "angular\\.module\\(" omega-web/src`.
- `omega-target-chromium-extension/` - WebExtension target + build; entry: `omega-target-chromium-extension/src/module/index.coffee`.
- `omega-locales/` - translation sources; search: `rg -n "message" omega-locales | head`.
- `omega-target-chromium-extension/overlay/` - packaged overlay assets; search: `rg -n "overlay" omega-target-chromium-extension`.
- `.circleci/` - CI config; search: `rg -n "omega-(pac|target)" .circleci`.
- `.github/` - issue/PR templates; search: `rg -n "ZeroOmega" .github`.

### Entry points

- `omega-build/package.json` - canonical dev/build/release commands; search: `rg -n "\"scripts\"" omega-build/package.json`.
- `omega-pac/index.coffee` - omega-pac public exports; search: `rg -n "module\\.exports" omega-pac/index.coffee`.
- `omega-pac/src/pac_generator.coffee` - PAC generation core; search: `rg -n "class PacGenerator" omega-pac/src/pac_generator.coffee`.
- `omega-target/index.coffee` - omega-target public exports; search: `rg -n "module\\.exports" omega-target/index.coffee`.
- `omega-target/src/options.coffee` - main options/profile logic; search: `rg -n "class Options" omega-target/src/options.coffee`.
- `omega-target/src/storage.coffee` - storage abstraction; search: `rg -n "class Storage" omega-target/src/storage.coffee`.
- `omega-target-chromium-extension/src/module/index.coffee` - extension wiring + re-exports from `omega-target`; search: `rg -n "for name, value of require\\('omega-target'\\)" omega-target-chromium-extension/src/module/index.coffee`.
- `omega-target-chromium-extension/src/coffee/background.coffee` - background script entry; search: `rg -n "background" omega-target-chromium-extension/src/coffee/background.coffee`.
- `omega-target-chromium-extension/src/coffee/omega_target_web.coffee` - browser-dependent bridge for `omega-web`; search: `rg -n "omegaTarget" omega-target-chromium-extension/src/coffee/omega_target_web.coffee`.

### Common tasks

- Install deps in all modules: `cd omega-build && npm run deps`
- Link local modules for dev: `cd omega-build && npm run dev`
- Build: `cd omega-build && npm run build`
- Release packages: `cd omega-build && npm run release`
- Run unit tests: `cd omega-pac && npm test` and `cd omega-target && npm test`

### Search cookbook

- All anchors: `rg -n "AICODE-"`.
- Only canonical prefixes: `rg -n "AICODE-(NOTE|TODO|CONTRACT|TRAP|LINK|ASK):"`.
- omega-pac core: `rg -n "class PacGenerator|compile\\:" omega-pac/src`.
- omega-pac profiles model: `rg -n "class Profiles|defaultProfile" omega-pac/src`.
- omega-target main Options: `rg -n "class Options" omega-target/src/options.coffee`.
- omega-target storage: `rg -n "class Storage|set\\:|get\\:" omega-target/src/storage.coffee`.
- omega-target sync: `rg -n "class OptionsSync" omega-target/src/options_sync.coffee`.
- extension background wiring: `rg -n "background\\.coffee|chrome\\." omega-target-chromium-extension/src/coffee`.
- extension proxy impl: `rg -n "proxy_impl|proxy_auth" omega-target-chromium-extension/src/module/proxy`.
- web UI entry points: `rg -n "angular\\.module\\(|\\.controller\\(" omega-web/src`.

Chromium Extension
------------------
The project is available as a Chromium Extension.

You can try it on [Chrome Web Store](https://chromewebstore.google.com/detail/pfnededegaaopdmhkdmcofjmoldfiped),
or grab a packaged extension file (CRX) for offline installation on the [Releases page](https://github.com/zero-peak/ZeroOmega/releases).

Please [report issues on the issue tracker](https://github.com/zero-peak/ZeroOmega/issues).

Firefox Addon
----------------------------

There is also a WebExtension port, which allows installing in Firefox. Compatibility with Firefox has increased significantly recently.

You can try it on [Mozilla Add-ons](https://addons.mozilla.org/en-US/firefox/addon/zeroomega/),
or grab a packaged extension file (XPI) for offline installation on the [Releases page](https://github.com/zero-peak/ZeroOmega/releases).

Please [report issues on the issue tracker](https://github.com/zero-peak/ZeroOmega/issues), browser-specific bugs are possible.

Development status
------------------

## PAC generator
This project contains a PAC generating module called `omega-pac`, which handles
the profiles model and compile profiles into PAC scripts. This module is standalone
and can be published to npm when the documentation is ready.

## Options manager
The folder `omega-target` contains browser-independent logic for managing the
options and applying profiles. Every public method is well documented in the comments.
Functions related to browser are not included, and shall be implemented in subclasses
of the `omega-target` classes.

`omega-web` is a web-based configuration interface for various options and profiles.
The interface works great with `omega-target` as the back-end.

`omega-web` alone is incomplete and requires a file named `omega_target_web.js`
containing an angular module `omegaTarget`. The module contains browser-dependent
code to communicate with `omega-target` back-end, and other code retrieving
browser-related state and information.
See the `omega-target-chromium-extension/omega_target_web.coffee` file for an
example of such module.

## Targets
The `omega-target-*` folders should contain environment-dependent code such as
browser API calls.

Each target folder should contain an extended `OmegaTarget` object, which
contains subclasses of the abstract base classes like `Options`. The classes
contains implementation of the abstract methods, and can override other methods
at will.

A target can copy the files in `omega-web` into its build to provide a web-based
configuration interface. If so, the target must provide the `omega_target_web.js`
file as described in the Options manager section.

Additionally, each target can contain other files and resources required for the
target, such as background pages and extension manifests.

For now, only one target has been implemented: The WebExtension target.
This target allows the project to be used as a Chromium extension in most
Chromium-based browsers and also as a Firefox Addon as mentioned above.

## Translation

Translation is hosted on Weblate. If you want to help improve the translated
text or start translation for your language, please follow the link of the picture
below.

本项目翻译由Weblate托管。如果您希望帮助改进翻译，或将本项目翻译成一种新的语言，请
点击下方图片链接进入翻译。

[![Translation status](https://hosted.weblate.org/widgets/switchyomega/-/287x66-white.png)](https://hosted.weblate.org/engage/switchyomega/?utm_source=widget)

## Building the project

ZeroOmega has migrated to use npm and grunt for building. Please note that
node 20.x is required for this project.

To build the project:

    # Install node and npm first (make sure node --version >= 20), then:
    
    # In the project folder:
    cd omega-build
    npm run deps # This runs npm install in every module.
    npm run dev # This runs npm link to aid local development.
    # Note: the previous command may require sudo in some environments.
    # The modules are now working. We can build now:
    npm run build
    # After building, a "build" folder will be generated in omega-target-chromium-extension folder.
    # The folder above can be loaded as an unpacked extension in Chromium now.
    npm run release
    # After release, a "dist" folder will be generated in root folder.
    cd ../dist # Return to generated folder.
    # The chromium-release.zip and firefox-release.zip are the package files corresponding to chromium and firefox respectively.

To enable `grunt watch`, run `grunt watch` once in the `omega-build` directory.
This will effectively run `grunt watch` in every module in this project.

License
-------
![GPLv3](https://www.gnu.org/graphics/gplv3-127x51.png)

ZeroOmega is licensed under [GNU General Public License](https://www.gnu.org/licenses/gpl.html) Version 3 or later.

ZeroOmega is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

ZeroOmega is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with ZeroOmega.  If not, see <http://www.gnu.org/licenses/>.

Notice
------


ZeroOmega currently does not have a dedicated project homepage. Please refer to this Github repository and wiki for official information.

ZeroOmega is not cooperating with any proxy providers, VPN providers or ISPs at the moment. No advertisement is displayed in ZeroOmega project or software. Proxy providers are welcome to recommend ZeroOmega as part of the solution in tutorials, but it must be made clear that ZeroOmega is an independent project, is not affiliated with the provider and therefore cannot provide any support on network connections or proxy technology.

重要声明
--------

ZeroOmega 目前没有专门的项目主页。一切信息请以 Github 上的项目和 wiki 为准。

ZeroOmega 目前未与任何代理提供商、VPN提供商或 ISP 达成任何合作协议，项目或软件中不包含任何此类广告。欢迎代理提供商在教程或说明中推荐 ZeroOmega ，但请明确说明此软件是独立项目，与代理提供商无关，且不提供任何关于网络连接或代理技术的支持。
