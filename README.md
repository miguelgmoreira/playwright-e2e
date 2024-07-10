# Comandos 

## Via CLI

- npx playwright test (_executa os testes_)
- npx playwright show-report (_exibe os relatórios do teste_)

### Para rodar os teste em um browser específico
- npx playwright test --project=chromium

### Para visualizar a execução do teste
- npx playwright test --project=chromium --headed

### Para executar os testes em um arquivo especifico
- npx playwright test example.spec.ts

### Para executar um caso de teste específico
- npx playwright test -g "has title"

### Para executar os testes em modo UI (fornece várias ferramentas)
- npx playwright test --ui

### Para gerar um relatório dos testes com evidencias
Por padrão o playwright gera os relatórios na primeira tentativa após um teste falhar

- npx playwright test --project=chromium --trace on

### Para debugar os testes
- npx playwright test --project=chromium --debug

## Via código

### Para pular um caso de teste

Na declaração do caso no código ao invés de _test()_ usamos _test.skip()_

```javascript
test.skip('has title', async ({ page }) => {
    await page.goto('https://playwright.dev/');

    await expect(page).toHaveTitle(/Playwright/);
});
```

### Para executar um caso de teste específico

Na declaração do caso no código ao invés de _test()_ usamos _test.only()_

```javascript
test.only('has title', async ({ page }) => {
    await page.goto('https://playwright.dev/');

    await expect(page).toHaveTitle(/Playwright/);
});
```

## Via extensão do vscode

- Permite executar os testes pelo próprio vscode e pode ser útil pois após finalizar os testes o browser continua aberto, enquanto ao executar pela CLI ele fecha após executar

- Link da extensão: https://marketplace.visualstudio.com/items?itemName=ms-playwright.playwright

## Para casos de teste que tenham como resposta uma Promise é necessário usar o async/await

```javascript
test.only('has title', async ({ page }) => {
    await page.goto('https://playwright.dev/');
});
```

## Hooks

- describe()
```javascript
test.beforeEach(() => {
   
});
```

- beforeEach()
```javascript
test.beforeEach(() => {
   
});
```
- beforeAll()
```javascript
test.beforeAll(() => {
   
});
```

### Obs: Não é considerado uma boa prática usar o afterEach e o afterAll

- afterEach()
```javascript
test.afterEach(() => {
   
});
```

- afterAll()
```javascript
test.afterAll(() => {
   
});
```

## User facing locators

- É considerado a melhor prática sempre que possível utilizar elementos visíveis ao usuário como localizador(locator) dos elementos html. Dentre eles estão:

getByRole()
```javascript
await page.getByRole('textbox', {name: 'Email'}).first().click();
await page.getByRole('button', {name: "Sign in"}).first().click();
```

getByLabel()
```javascript
await page.getByLabel('Email').first().click();
```

getByPlaceholder()
```javascript
await page.getByPlaceholder('Jane Doe').click();
```

getByText()
```javascript
await page.getByText('Using the Grid').click();
```

getByTestId()
```javascript
await page.getByTestId('SignIn').click()
```

getByTitle()
```javascript
await page.getByTitle('IoT Dashboard').click();
```

## Localizar elementos filhos
```javascript
await page.locator('nb-card nb-radio :text-is("Option 1")').click();
await page.locator('nb-card').locator('nb-radio').locator(':text-is("Option 2)').click();

await page.locator('nb-card').getByRole('button', {name: 'Sign in'}).first().click();

await page.locator('nb-card').nth(3).getByRole('button').click();

// Evitar usar nth(index), first e last. Tentar ser o mais específico possível a fim de evitar erros.
```

## Localizar elementos pais
```javascript
await page.locator('nb-card', {hasText: "Using the Grid"}).getByRole('textbox', {name: 'Email'}).click();
await page.locator('nb-card', {has: page.locator('#inputEmail')}).getByRole('textbox', {name: "Email"}).click();

await page.locator('nb-card').filter({hasText: "Basic form"}).getByRole('textbox', {name: "Email"}).click();
await page.locator('nb-card').filter({has: page.locator('.status-danger')}).getByRole('textbox', {name: "Email"}).click();

await page.locator('nb-card').filter({has: page.locator('nb-checkbox')}).filter({hasText: "Sign in"}).getByRole('textbox', {name: "Email"}).click();
```