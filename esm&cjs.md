esm 导出：
export const a = 0
export default 0
//export const default = 0
cjs 导出：
module.exports.a = 0
module.exports.default = 0

esm 导出：
import a, { b } from "m"
//import { default as a, b } from "m"
cjs 导出：
const { default: a, b } = require("m")

esm 导入整个模块：
import * as m from "m"
cjs 导入整个模块：
const m = require("m")

typescript 导出整个模块，假装 esm 也支持：
export = 0
cjs 导出整个模块：
module.exports = 0
