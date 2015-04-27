# Moip 2

Typescript para Moip API 2.0

## Install

```
$ npm install @pocesar/moip2 --save
```

## Typescript

```typescript
/* se não tiver o bluebird nos typings */
/// <reference path="node_modules/@pocesar/moip2/typings/bluebird/bluebird.d.ts" />
/* ou tudo */
/// <reference path="node_modules/@pocesar/moip2/typings/tsd.d.ts" />
/* typings especificos do MOIP */
/// <reference path="node_modules/@pocesar/moip2/moip.d.ts" />
```

## Usage

```js
var Moip = require('@pocesar/moip2').Moip;

var instance = new Moip('token', 'chave', true);

instance.createCustomer({
    birthDate: '0000-00-00',
    email: 'email@example.com',
    fullname: 'Full Name',
    ownId: 'ownUserId',
    phone: {
        areaCode: '00',
        countryCode: '00',
        number: '00000000'
    },
    taxDocument: {
        number: '00000000',
        type: 'CPF'
    },
    shippingAddress:{
        city: 'Cidade',
        complement: 'Complemento',
        country: 'BRA',
        district: 'Bairro',
        state: 'XX',
        street: 'RUA',
        streetNumber: 'NUMERO',
        zipCode: '00000000'
    }
}).then(function(customer){
    delete customer._links;

    return this.createOrder({
        amount: {
            currency: 'BRL',
            subtotals: {}
        },
        customer: customer,
        items: [{
            detail: '',
            price: 50000,
            product: 'Compra',
            quantity: 1
        }],
        ownId: 'ownOrderId'
    });
}).then(function(order){

    return this.createPayment({
        fundingInstrument: {
            method: 'BOLETO',
            boleto: {
                expirationDate: '2015-05-12',
                instructionLines: {
                    first: 'first',
                    second: 'second',
                    third: 'thid'
                }
            }
        }
    }, order.id);
}).then(function(payment){
    console.log('Sucesso!', payment);
}).catch(console.error.bind(console));
```

## License

GPLv3