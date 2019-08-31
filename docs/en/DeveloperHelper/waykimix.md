<extoc></extoc>
# WaykiMix
## Introdution
`WaykiMix is` a web smart contract IDE based on vue.js language. It has the functions of smart contract development, deployment, invocation, debugging, etc. It can provide a great convenience for developers to develop smart contract based on WaykiChain Lua language.

## User guidance
### Install WaykiMax
[Install][1]

### Load WaykiMix
open link [https://waykimix.wiccdev.org][2] enter the WaykiMix web page

### Home Layout
The left side of the WaykiMix operator interface is File Management, which lists the files in the current workspace. The WaykiMix tool supports functions such as reading files locally, saving files to locally, and creating new files.

The middle part is the work area. The upper part of the workspace is the code editing area, which can be written based on Lua-based smart contract. The lower part is the Log output area, which is convenient for developers to debug.

The right sidebar is a ribbon that provides functional sections including deployment, calls, tools, and help.
![][image-1]

### Ribbon-Deploy
Click the `Refresh` button to refresh the network, address, balance, etc. of the wallet in `WaykiMax`.

Click the `Deploy` button to deploy the smart contract with one click. At this time, call `WaykiMax` to sign and click `OK`.
![][image-2]

### 功能区-Run
After deploying the smart contract via `Deploy`, there is a transaction hash in the `Run` section when the contract is released.
![][image-3]

After waiting for 10 seconds for the transaction to be confirmed, click the `Get` button to get the regid of the smart contract.

Through the Param area, according to the specific contract content, the information that needs to be called when the contract is called

Click the `Gen` button to start the stitching. The information you need to call when you call the contract.
![][image-4]

Continue to click the `Call` button to initiate the call contract transaction. At this time, call `WaykiMax` to sign, click `OK`, and the transaction log of the calling contract is displayed in the Log area.
![][image-5]

### 功能区-Tool
The Tool section provides `hexadecimal text to decode` and `hex text decode to bytes` functions. For details, please refer to the example.
![][image-6]

### Ribbon - Help
The Help section provides links to how-to guides, developer contact information, version numbers, and more.
![][image-7]

## Open Sourcecode

https://gitlab.com/waykichain-public/wicc-tools/wicc-waykimix

[1]:	webextension.md
[2]:	https://waykimix.wiccdev.org

[image-1]:	../images/waykimix1.png
[image-2]:	../images/waykimix2.png
[image-3]:	../images/waykimix3.png
[image-4]:	../images/waykimix4.png
[image-5]:	../images/waykimix5.png
[image-6]:	../images/waykimix6.png
[image-7]:	../images/waykimix7.png
