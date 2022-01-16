#  Github 레포지토리 CLI 

commandline interface 지원해주는 라이브러리 사용

## Installer
```javascript
const { program } = require('commander')

program.version('0.0.1')
```

## Example
```javascript
program
  .option('-d, --debug', 'output extra debugging')
  .option('-s, --small', 'small pizza size')
  .option('-p, --pizza-type <type>', 'flavour of pizza')

program.parse(process.argv)

const options = program.opts()
if (options.debug) console.log(options)
console.log('pizza details:')
if (options.small) console.log('- small pizza size')
if (options.pizzaType) console.log(`- ${options.pizzaType}`)
```

###   issue, pull request등의 관리