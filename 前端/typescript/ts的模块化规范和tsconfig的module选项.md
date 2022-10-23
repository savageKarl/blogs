# ts的模块化规范和tsconfig的module选项

`tsconfig.json`的`compilerOptions`选项`module`属性的配置，这个属性表示通过tsc编译后的js文件将采取哪种模块化规范。

在`.ts`文件的编码中，typescript 支持 

ESM的规范

```typescript
import {xxxx,xxxx} from 'xxxxx'
import xxxx from 'xxxx'
import * as xxx from 'xxxx'

export default xxxx;
export const xxxxx;
export * from 'xxxxx';
```



CommonJs的规范

```js
const xxxx = require('xxxx');
const {xxxx}  = required('xxxx');

module.exports = xxxxxx
```



同时兼容 AMD 规范 和 CommonJs的规范的奇葩规范

```javascript
import xxxxx = require('xxxx');

export =
```

