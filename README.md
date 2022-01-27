#  Github Repositories CLI 

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
## 'me' commander
```javascript
program
  .command('me')
  .description('Check my profile')
  .action(async () => {
    const {
      data: { login },
    } = await octokit.rest.users.getAuthenticated()
    console.log('Hello, %s', login)
  })
```
octokit library 사용 , 저장소 접근
###   issue, pull request등의 관리