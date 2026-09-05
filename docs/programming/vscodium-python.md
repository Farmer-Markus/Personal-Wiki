# Problems with python in vscodium
## Getting auto-complete to work
Use the [Basedpyright](https://open-vsx.org/extension/detachhead/basedpyright) extension and add the following to your VSCodium user settings **(shift+ctrl+p: "Preferences: Open User Settings (JSON)")** <br>
(Add after possible existing entries)
``` json
{
	"basedpyright.importStrategy": "useBundled"
}
```

