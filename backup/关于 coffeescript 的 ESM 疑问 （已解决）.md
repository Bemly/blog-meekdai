```
#!/usr/bin/env coffee
import * as cfs from "coffeescript"
```

```
 coffee index
(node:44936) Warning: To load an ES module, set "type": "module" in the package.json or use the .mjs extension.
(Use `node --trace-warnings ...` to show where the warning was created)
/home/bemly/www/luoli-parse-toolchain/src/index:4
import * as cfs from "coffeescript";
^^^^^^

SyntaxError: Cannot use import statement outside a module
    at wrapSafe (node:internal/modules/cjs/loader:1469:18)
    at Module._compile (node:internal/modules/cjs/loader:1491:20)
    at CoffeeScript.run (/usr/lib/node_modules/coffeescript/lib/coffeescript/index.js:67:23)
    at compileScript (/usr/lib/node_modules/coffeescript/lib/coffeescript/command.js:285:29)
    at compilePath (/usr/lib/node_modules/coffeescript/lib/coffeescript/command.js:237:14)
    at exports.run (/usr/lib/node_modules/coffeescript/lib/coffeescript/command.js:158:20)
    at Object.<anonymous> (/usr/lib/node_modules/coffeescript/bin/coffee:22:45)
    at Module._compile (node:internal/modules/cjs/loader:1546:14)
    at Module._extensions..js (node:internal/modules/cjs/loader:1691:10)
    at Module.load (node:internal/modules/cjs/loader:1317:32)
    at Module._load (node:internal/modules/cjs/loader:1127:12)
    at TracingChannel.traceSync (node:diagnostics_channel:315:14)
    at wrapModuleLoad (node:internal/modules/cjs/loader:217:24)
    at Function.executeUserEntryPoint [as runMain] (node:internal/modules/run_main:166:5)
    at node:internal/main/run_main_module:30:49
```

```
#!/usr/bin/env node
import * as cfs from "coffeescript"
```
it WORK!!!

而现在的node已经可以正常的识别ESM模式的js了，coffee不知道为什么还没有支持，为什么转义可以识别

source: https://coffeescript.org/#modules

官方的解释：

Note that the CoffeeScript compiler does not resolve modules; writing an import or export statement in CoffeeScript will produce an import or export statement in the resulting output. Such statements can be run by all modern browsers (when the script is referenced via <script type="module">) and [by Node.js](https://nodejs.org/api/esm.html#esm_enabling) when the output .js files are in a folder where the nearest parent package.json file contains "type": "module". Because the runtime is evaluating the generated output, the import statements must reference the output files; so if file.coffee is output as file.js, it needs to be referenced as file.js in the import statement, with the .js extension included.

注意，CoffeeScript编译器不会解析模块;在CoffeeScript中编写import或export语句将在结果输出中生成import或export语句。所有现代浏览器都可以运行这样的语句（当脚本通过<script type="module">引用时），当输出的.js文件位于最近的父package.json文件包含"type": "module"的文件夹中时,[Node.js](https://nodejs.org/api/esm.html#esm_enabling)可以运行这样的语句。由于运行时正在评估生成的输出，因此import语句必须引用输出文件;因此如果file.coffee输出为file.js，则需要在file.js语句中引用为import，并包括.js扩展。

我尝试了 coffee index.mjs和  coffee index.js ，但是均是CommonJS模式，不知道为什么。

![图片](https://github.com/user-attachments/assets/0045b604-5a67-4ba8-b274-398a1f2e11e6)
目前 vm 模块还缺乏 ESM 支持，所以不行 噗

顺便安利一下最近新挖的坑：https://github.com/Bemly/luolita

如果你也喜欢用 **这种没有符号** 的优雅单文件组件SFC写法，欢迎提交 pr 加入代码开发，成为贡献者！