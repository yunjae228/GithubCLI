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

##   issue, pull request등의 관리
pr을 검사해서, 만약 너무 diff가 크다면 `too-big`labels을 붙인다

```javascript
program
  .command('check-prs')
  .description('Check pull request status')
  .action(async () => {
    const result = await octokit.rest.pulls.list({
      owner: OWNER,
      repo: REPO,
    })

    const prsWithDiff = await Promise.all(
      result.data.map(async (pr) => ({
        number: pr.number,
        compare: await octokit.rest.repos.compareCommits({
          owner: OWNER,
          repo: REPO,
          base: pr.base.ref,
          head: pr.head.ref,
        }),
      }))
    )
    console.log(prsWithDiff.map((diff) => diff.compare.data.files))
  })
```
