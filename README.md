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

      //코드중복 방지
    })

    const prsWithDiff = await Promise.all(
      result.data.map(async (pr) => ({
        number: pr.number, //pull requests 넘버링
        //커밋 비교
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
# 커밋을 비교할 수 있게 octokit 라이브러리를 활용하여 map()함수로 새로 배열

```javascript
.map(({ compare, ...rest }) => {
          const totalChanges = compare.data.files?.reduce(
            (sum, file) => sum + file.changes,
            0
          )
```
# data/files의 diff를 비교하기 위해 totalChanges변수를 새로 만들어준다


```javascript
.map(async ({ labels, number }) => {
          if (!labels.find((label) => label.name === LABEL_TOO_BIG)) {
            console.log(chalk.green(`PR ${LABEL_TOO_BIG} label to PR ${number}...`))
```
# diff가 100이상이 넘어갈때 녹색으로 콘솔창에서 알려준다 
하지만 chalk 라이브러리가 ESM의 import/export 방식만을 지원하여 사용 할 수 없었다

# color 라이브러리로 대체 
```javascript
.map(async ({ labels, number }) => {
          if (!labels.find((label) => label.name === LABEL_TOO_BIG)) {
            console.log(colors.green(`PR ${LABEL_TOO_BIG} label to PR ${number}...`))
```
# console.log에 green색상 들어가는 것 확인
<img width="516" alt="screenshot 2022-02-17 at 2 55 05 pm" src="https://user-images.githubusercontent.com/74397919/154414335-aa621b6f-f0f0-4866-b7be-76d579f21420.png">

# 실행 후 pull request에 too-big labels이 붙여짐
<img width="377" alt="screenshot 2022-02-17 at 2 58 34 pm" src="https://user-images.githubusercontent.com/74397919/154414696-52a3def3-bd26-4511-9f11-c309022db94b.png">

