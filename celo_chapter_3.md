---
title: 3. Building the Frontend of the DApp
description: In this chapter, you will learn how to build the user interface of your DApp, connect it to your smart contract and host it on GitHub.
objectives:
  - Learn how to write the HTML and JS part of your DApp.
  - Connect your DApp to your smart contract on the Celo blockchain with the library ContractKit.
  - Learn how to enable purchases via the Celo stablecoin cUSD.
---

Here is a preview of the DApp that you are going to build:
[Link to Marketplace DApp](https://moritzfelipe.github.io/celo-marketplace-dapp/)

## 3.1 Initializing your project (3 min)
First, you are going to initialize your project from our boilerplate repo, where you will already have the build process and necessary libraries available. 
You should have installed node.js 10 or higher.

1. Open your command-line interface.
2. Clone our boilerplate repository.
```
git clone https://github.com/moritzfelipe/celo-boilerplate-web-dapp
```

3. Navigate to the new repository.
```
cd celo-boilerplate-web-dapp
```

4. Install all dependencies.
```
npm install
```

5. Start a local development server.
```
npm run dev
```

Your project should be running here http://localhost:3000/.

## 3.2 The HTML of your DApp (9 min)

In this part of the tutorial, you are going to start the basis of your DApp, the HTML.

Although this part may look long, you will quickly go through it, since most code in here is pretty trivial. Don’t worry about all the classes they are just for styling purposes.

Open the index.html file inside the public folder of your project.

Ok, now you get started with the document.


```
<!DOCTYPE html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
```


You declare the document type, add an HTML tag, create a head element and add meta tags.

Next, you are going to import some external stylesheets.

```
    <!-- CSS -->
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/css/bootstrap.min.css"
      rel="stylesheet"
      integrity="sha384-BmbxuPwQa2lc/FVzBcNJ7UAyJxM6wuqIj61tLrc4wSX0szH/Ev+nYRRuWlolflfl"
      crossorigin="anonymous"
    />
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.4.0/font/bootstrap-icons.css"
    />
```

In this tutorial, you are going to use bootstrap which is a popular front-end library that allows you to create elegant responsive websites very fast. You can quickly choose from bootstrap components like buttons or cards and customize them to your need ([Learn more about Bootstrap Components](https://getbootstrap.com/docs/5.0/components/)). This is what we have done for this tutorial.

You also import the bootstrap icons.

```
    <link rel="preconnect" href="https://fonts.gstatic.com" />
	<link
	  href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;700&display=swap"
	  rel="stylesheet"
	/>
```

You add a font that you want to use for this DApp, in this case, it’s DM Sans.

```
	<style>
	  :root {
		--bs-font-sans-serif: "DM Sans", sans-serif;
	  }

	  @media (min-width: 576px) {
		.card {
		  border: 0;
		  box-shadow: rgb(0 0 0 / 5%) 0px 10px 20px;
		  border-radius: 10px;
		}

		.card-img-top {
		  width: 100%;
		  height: 20vw;
		  object-fit: cover;
		}
	  }
	</style>

	<title>Street Food Kigali</title>
	</head>
```

You import the font as your main font and adjust the style of Bootstrap cards component for bigger screens, you will use them for your products later.

In the end, you add a title.

```
  <body>
    <div class="container mt-2" style="max-width: 72em">

      <nav class="navbar bg-white navbar-light">
        <div class="container-fluid">
          <span class="navbar-brand m-0 h4 fw-bold">Street Food Kigali</span>
          <span class="nav-link border rounded-pill bg-light">
            <span id="balance">0</span>
            cUSD
          </span>
        </div>
      </nav>
```

Inside the body element, you create a navigation bar, where you display the name of the DApp and the cUSD balance of the user.

You will get the value for the user’s balance later from your Javascript and display it in the span element with the id `balance`.

```
  <div class="alert alert-warning sticky-top mt-2" role="alert">
    <span id="notification">⌛ Loading...</span>
  </div>
```

You create a `div` for the notifications that you want to display to the user. This div has the class alert, you are going to select it later in your JS code. The `span` element has the id `notification`, that you are going to use to insert the text that you want to display.

```
  <div class="mb-4" style="margin-top: 4em">
    <a
      class="btn btn-dark rounded-pill"
      data-bs-toggle="modal"
      data-bs-target="#addModal"
    >
      Add product
    </a>
  </div>
```

You create a div to display the `Add product` button at the right place. The `Add product` button will toggle a modal that will allow the user to add a new product to the DApp.

```
  <main id="marketplace" class="row"></main>
</div>
```

Finally, you will add a main tag with the id marketplace where you will display your products later.

You will also need to create the modal that opens up when the user clicks on the `Add product` button. This code is pretty long, but not much is happening here.

```
<!--Modal-->
<div
  class="modal fade"
  id="addModal"
  tabindex="-1"
  aria-labelledby="newProductModalLabel"
  aria-hidden="true"
>
  <div class="modal-dialog">
	<div class="modal-content">

	  <div class="modal-header">
		<h5 class="modal-title" id="newProductModalLabel">New Product</h5>
		<button
		  type="button"
		  class="btn-close"
		  data-bs-dismiss="modal"
		  aria-label="Close"
		></button>
	  </div>
```

You create a modal with a title and a close button.
```
      <div class="modal-body">
        <form>
          <div class="form-row">
            <div class="col">
              <input
                type="text"
                id="newProductName"
                class="form-control mb-2"
                placeholder="Enter name of product"
              />
            </div>
            <div class="col">
              <input
                type="text"
                id="newImgUrl"
                class="form-control mb-2"
                placeholder="Enter image url"
              />
            </div>
            <div class="col">
              <input
                type="text"
                id="newProductDescription"
                class="form-control mb-2"
                placeholder="Enter product description"
              />
            </div>
            <div class="col">
              <input
                type="text"
                id="newLocation"
                class="form-control mb-2"
                placeholder="Enter location"
              />
            </div>
            <div class="col">
              <input
                type="text"
                id="newPrice"
                class="form-control mb-2"
                placeholder="Enter price"
              />
            </div>
          </div>
        </form>
      </div>
```

In this part of the modal, you are creating a form, so that the user can input the attributes of the product they want to add to your DApp. They need to enter the name, image URL, product description, location and price of their product.

```
    <div class="modal-footer">
        <button
          type="button"
          class="btn btn-light border"
          data-bs-dismiss="modal"
        >
          Close
        </button>
        <button
          type="button"
          class="btn btn-dark"
          data-bs-dismiss="modal"
          id="newProductBtn"
        >
          Add product
        </button>
      </div>
    </div>
  </div>
</div>
<!--/Modal-->
```


In the footer of your modal, you add another Close button and an `Add product` button with the id `newProductBtn`.

```
    <script
    src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/js/bootstrap.bundle.min.js"
    integrity="sha384-b5kHyXgcpbZJO/tY9Ul7kGkf1S0CWuKcCD38l8YkeH8z8QjE0GmW1gYU5S9FOnJ0"
    crossorigin="anonymous"
    ></script>
    <script src="https://unpkg.com/ethereum-blockies@0.1.1/blockies.min.js"></script>
  </body>
</html>
```

Finally, you add the bootstrap JS library and a library called blockies, that you are going to use to visualise blockchain addresses.

Congratulations! That’s it for the first part of this tutorial.

Here is a quick view of how your DApp should look like with just the HTML now.
![](https://cdn.kapwing.com/final_60a12cdeafd79300445c7cff_668803.gif)

HTML Code
[Link to code](https://github.com/moritzfelipe/celo-development-101-data/blob/main/code/3-building-the-frontend-of-the-dapp/3-2-the-html-of-your-project/index.html)

## 3.3 The JS of your DApp (12 min)
In this part of the tutorial, you will add the basic functionality of your DApp. You will create a functioning version in vanilla Javascript with sample products, that is not connected to the Celo blockchain yet.

Open the main.js file inside the src folder of your project.
```

const products = [
  {
    name: "Giant BBQ",
    image: "https://i.imgur.com/yPreV19.png",
    description: `Grilled chicken, beef, fish, sausages, bacon, 
      vegetables served with chips.`,
    location: "Kimironko Market",
    owner: "0x32Be343B94f860124dC4fEe278FDCBD38C102D88",
    price: 3,
    sold: 27,
    index: 0,
  },
  {
    name: "BBQ Chicken",
    image: "https://i.imgur.com/NMEzoYb.png",
    description: `French fries and grilled chicken served with gacumbari 
      and avocados with cheese.`,
    location: "Afrika Fresh KG 541 St",
    owner: "0x3275B7F400cCdeBeDaf0D8A9a7C8C1aBE2d747Ea",
    price: 4,
    sold: 12,
    index: 1,
  },
  {
    name: "Beef burrito",
    image: "https://i.imgur.com/RNlv3S6.png",
    description: `Homemade tortilla with your choice of filling, cheese, 
      guacamole salsa with Mexican refried beans and rice.`,
    location: "Asili - KN 4 St",
    owner: "0x2EF48F32eB0AEB90778A2170a0558A941b72BFFb",
    price: 2,
    sold: 35,
    index: 2,
  },
  {
    name: "Barbecue Pizza",
    image: "https://i.imgur.com/fpiDeFd.png",
    description: `Barbecue Chicken Pizza: Chicken, gouda, pineapple, onions 
      and house-made BBQ sauce.`,
    location: "Kigali Hut KG 7 Ave",
    owner: "0x2EF48F32eB0AEB90778A2170a0558A941b72BFFb",
    price: 1,
    sold: 2,
    index: 3,
  },
]
```

Since your DApp is not connected to your contract where you store the products, you create an array called products, with sample product data.

```
const getBalance = function () {
  document.querySelector("#balance").textContent = 21
}
```

In the `getBalance` function, you update the balance of the user. In this static version you hard code the number 21 in it, so that in the HTML span element with the id balance, 21 will be displayed.

```
function renderProducts() {
  document.getElementById("marketplace").innerHTML = ""
  products.forEach((_product) => {
    const newDiv = document.createElement("div")
    newDiv.className = "col-md-4"
    newDiv.innerHTML = productTemplate(_product)
    document.getElementById("marketplace").appendChild(newDiv)
  })
}
```

Next, you want to display the products with the renderProducts function. 

First, you empty your marketplace div, so that you don’t add the products multiple times when you call `renderProducts` more than once.

For each product in products, you are going to create a new div with the HTML of productTemplate, that you are going to add next, and at the end, you append the new div to your marketplace div.

Now you create the HTML of the product.


```
function productTemplate(_product) {
  return `
    <div class="card mb-4">
      <img class="card-img-top" src="${_product.image}" alt="...">
      <div class="position-absolute top-0 end-0 bg-warning mt-4 px-2 py-1 rounded-start">
        ${_product.sold} Sold
      </div>
```

In your `productTemplate` function, you create a card with an image of the product and a little box with the number of times it has been sold.

```

      <div class="card-body text-left p-4 position-relative">
        <div class="translate-middle-y position-absolute top-0">
        ${identiconTemplate(_product.owner)}
        </div>
        <h2 class="card-title fs-4 fw-bold mt-2">${_product.name}</h2>
        <p class="card-text mb-4" style="min-height: 82px">
          ${_product.description}             
        </p>
        <p class="card-text mt-4">
          <i class="bi bi-geo-alt-fill"></i>
          <span>${_product.location}</span>
        </p>
        <div class="d-grid gap-2">
          <a class="btn btn-lg btn-outline-dark buyBtn fs-6 p-3" id=${
            _product.index
          }>
            Buy for ${_product.price} cUSD
          </a>
        </div>
      </div>
    </div>
  `
}

```

You create a small image called an identicon which has its own `identiconTemplate`. An identicon is a visual representation of a hash value, in our case of the address of the product owner. This makes it easy for people to identify, which owner created what products.

You also display the product name, description, location and at the end the price displayed on the buy button. The product index will be the id of the button element, so you know what product the user selected when they click on it.


```
function identiconTemplate(_address) {
  const icon = blockies
    .create({
      seed: _address,
      size: 8,
      scale: 16,
    })
    .toDataURL()

  return `
  <div class="rounded-circle overflow-hidden d-inline-block border border-white border-2 shadow-sm m-0">
    <a href="https://alfajores-blockscout.celo-testnet.org/address/${_address}/transactions"
        target="_blank">
        <img src="${icon}" width="48" alt="${_address}">
    </a>
  </div>
  `
}
```

Now you are going to create the identicon. The `identiconTemplate` function takes an address as a parameter and then creates an icon object with the address through the blockies library.

You return a round image of the icon and a link that takes the user to the transactions of the address.


```
function notification(_text) {
  document.querySelector(".alert").style.display = "block"
  document.querySelector("#notification").textContent = _text
}

function notificationOff() {
  document.querySelector(".alert").style.display = "none"
}
```

You create a `notification` function that displays the alert element with the text in the parameter and a `notificationOff` function that stops showing the alert element.

Now you create some event handlers.

```
window.addEventListener("load", () => {
  notification("⌛ Loading...")
  getBalance()
  renderProducts()
  notificationOff()
})
```

When your DApp is loaded you want to call your notification function with a loading message, you display the user's balance, render all products so the user can see them and disable the notification div again.

```
document
  .querySelector("#newProductBtn")
  .addEventListener("click", () => {
    const _product = {
      owner: "0x2EF48F32eB0AEB90778A2170a0558A941b72BFFb",
      name: document.getElementById("newProductName").value,
      image: document.getElementById("newImgUrl").value,
      description: document.getElementById("newProductDescription").value,
      location: document.getElementById("newLocation").value,
      price: document.getElementById("newPrice").value,
      sold: 0,
      index: products.length,
    }
    products.push(_product)
    notification(`🎉 You successfully added "${_product.name}".`)
    renderProducts()
  })
```

When a user clicks on the `newProductBtn` inside the modal, you get all values from the form.

For the first value, you hard code the address of the owner because it would be too much work to randomly create an address just for this example.

Then you get the values from the input fields for name, image, description, location, price and price. 

You set `sold` to 0 because the product is brand new and the index to the length of your product array. You start with 0 products so your first product will have the index 0, perfect.

You add the new product to your products array, send a notification and render the products.

```
document.querySelector("#marketplace").addEventListener("click", (e) => {
  if(e.target.className.includes("buyBtn")) {
    const index = e.target.id
    products[index].sold++
    notification(`🎉 You successfully bought "${products[index].name}".`)
    renderProducts()
  }
})
```

If the user clicks on the buy button of the product, you first save the index of the product they clicked on and increase the amount the item has been sold. You send them a notification and render products again, so the new sold amount is displayed updated.

That’s it for this part, this is how your app should behave now:
![](https://cdn.kapwing.com/final_60a21573ddb24a0104d62f8c_737613.gif)

Javascript Code
[Link to code](https://github.com/moritzfelipe/celo-development-101-data/blob/main/code/3-building-the-frontend-of-the-dapp/3-3-the-js-of-your-dapp/index.js)

## 3.4 Reading your Celo Balance (14min)
In this part of the tutorial, you are going to use the contractKit library to interact with the Celo Blockchain. ContractKit includes web3.js, a very popular collection of libraries also used for ethereum, that allows you to get access to a web3 object and interact with node's JSON RPC API ([Learn more about contractKit](https://docs.celo.org/developer-guide/contractkit)).

In this part, you will establish a connection to the Celo Blockchain and read out the cUSD balance that your connected account has.

Open the main.js that you just worked on in the last step and expand it.
But first, get rid of the products array, you don’t need sample data anymore.

```
import Web3 from 'web3'
import { newKitFromWeb3 } from '@celo/contractkit'
import BigNumber from "bignumber.js"

const ERC20_DECIMALS = 18

let kit
```

Import the `newKitFromWeb3`, `Web3` and `BigNumber` objects from their respective libraries.

Many Celo operations operate on numbers that are outside the range of values to use in Javascript. You use `bignumber.js` because it allows you to handle these numbers.

You create the variable `ERC20_DECIMALS`  and set it to 18. By default, the ERC20 interface uses a value of 18 for decimals. Since cUSD, the Celo currency that you are going to use here is an ERC20 token, you use `ERC20_DECIMALS` to convert values.

Declare also a global variable for contractKit, since you're going to use it in multiple functions.

Now you create a function to connect to the Celo extension wallet.
```
const connectCeloWallet = async function () {
  if (window.celo) {
      notification("⚠️ Please approve this DApp to use it.")
    try {
      await window.celo.enable()
      notificationOff()

      const web3 = new Web3(window.celo)
      kit = newKitFromWeb3(web3)

    } catch (error) {
      notification(`⚠️ ${error}.`)
    }
  } else {
    notification("⚠️ Please install the CeloExtensionWallet.")
  }
}
```

You create a new asynchronous function called `connectCeloWallet` that you will use to connect to the Celo Blockchain and read the balance of the user.

First, you check if the user has installed the CeloExtensionWallet, you do that by checking if the “window.celo” object exists. If it doesn’t you let them know with a notification that they need to install the wallet.

If it is, you send them a notification that they need to approve this DApp and try the window.celo.enable() function. That opens a pop-up dialogue in the UI and asks for permission to connect the DApp to the CeloExtensionWallet. After celo is enabled you disable the notification.

If you catch an error you let the user know that they must approve the dialogue to use the DApp.

Once the user approves the DApp you create a web3 object using the window.celo object as the provider.
With this web3 object, you can create a new kit instance that you save to the global kit variable. Now you have the functionality of the connected kit to interact with the Celo Blockchain.

Next, you will access the account of the user.

```
const connectCeloWallet = async function () {
  if (window.celo) {
    notification("⚠️ Please approve this DApp to use it.")
    try {
      await window.celo.enable()
      notificationOff()

      const web3 = new Web3(window.celo)
      kit = newKitFromWeb3(web3)

      const accounts = await kit.web3.eth.getAccounts()
      kit.defaultAccount = accounts[0]

    } catch (error) {
      notification(`⚠️ ${error}.`)
    }
  } else {
    notification("⚠️ Please install the CeloExtensionWallet.")
  }
}
```

Your `connectCeloWallet` function stays basically the same, but beneath your new kit instance you call the method, `kit.web3.eth.getAccounts()`, that returns an array of addresses of the connected user. In the case of the CeloExtensionWallet, you get one address and you can make it kit’s default address of the user with `kit.defaultAccount`, so you can use it globally.

Now you need to adjust your getBalance function to access and display the cUSD balance of the user.

```
const getBalance = async function () {
  const totalBalance = await kit.getTotalBalance(kit.defaultAccount)
  const cUSDBalance = totalBalance.cUSD.shiftedBy(-ERC20_DECIMALS).toFixed(2)
  document.querySelector("#balance").textContent = cUSDBalance
}
```
You create an asynchronous function `getBalance`.
With kit’s method `kit.getTotalBalance` you can get the total balance of an account, in this case, you want the balance from the account that interacts with the CeloExtensionWallet, the one we stored in `kit.defaultAccount`.

In the next line, you get a readable cUSD balance. You use totalBalance.cUSD.to get the cUSD balance. Since it’s a big number you convert it to be readable by shifting the comma 18 places to the left and use `toFixed(2)` to only get two decimal places after the comma.

Instead of 14000000000000000000, you get 14.00.

You display the `cUSDBalance` in the corresponding HTML element.

Finally, you need to call your new functions when the page is loaded, adjust your event listener for that.

```
window.addEventListener('load', async () => {
  notification("⌛ Loading...")
  await connectCeloWallet()
  await getBalance()
  notificationOff()
});
```

You make this function asynchronous and you call `connectCeloWallet()` and `getBalance()` with await.

Great job!

Now you can test your DApp and see if it can read out your balance.

We assume that you have installed the CeloExtensionWallet, if you haven’t you need to:
1. Install the [CeloExtensionWallet](https://chrome.google.com/webstore/detail/celoextensionwallet/kkilomkmpmkbdnfelcpgckmpcaemjcdh?hl=en) from the google chrome store.
2. Create a wallet.
3. Go to [https://celo.org/developers/faucet](https://celo.org/developers/faucet) and get some token for the alfajores testnet.
4. Switch to the alfajores testnet in the CeloExtensionWallet.
5. Open your DApp.

Here is a GIF showing how your app should behave now.
![](https://cdn.kapwing.com/final_60a22a2b7e73120137c98b5e_564180.gif)

Code for this part of the tutorial.
[Link to code](https://github.com/moritzfelipe/celo-development-101-data/blob/main/code/3-building-the-frontend-of-the-dapp/3-4-reading-your-celo-balance/index.js)

## 3.5 Read and write to your contract (12 min)
In this part of the tutorial, you will connect to your contract, and read and write products to it.

In order to interact with your smart contract that is deployed in bytecode, you need an interface, the ABI (Application Binary Interface), so that the contractKit can understand the bytecode. Through the ABI you call functions and read data ([Learn more about the ABI](https://docs.soliditylang.org/en/develop/abi-spec.html)).

When you compile your contract in Remix, Remix also creates the ABI in the form of a JSON for your contract. Copy it and save it into the marketplace.abi.json file of the contracts folder in your project.

![](https://cdn.kapwing.com/final_60a2319c3c6fc1010275a55a_325034.gif)

Now that you have your ABI saved in your project you need to import it.

```
import Web3 from 'web3'
import { newKitFromWeb3 } from '@celo/contractkit'
import BigNumber from "bignumber.js"
import marketplaceAbi from '../contract/marketplace.abi.json'

```

You import this ABI for our marketplace contract and call it `marketplaceAbi`.

After the deployment of your marketplace contract, you got the address of the contract, you need this address in order to find your contract and interact with it.

```
const ERC20_DECIMALS = 18
const MPContractAddress = "0x178134c92EC973F34dD0dd762284b852B211CFC8"
```

You save the address for the marketplace contract in the global variable `MPContractAddress`.
This is the address of my contract, you replace it with your own.

![](https://cdn.kapwing.com/final_60a2392a7e73120137c9ade0_359354.gif)

```
let kit
let contract
let products = []
```

You also declare two new variables, `contract` and `products`, which is an empty array.

When the user connects their celo wallet you will create an instance of the marketplace contract so you can interact with it.

```
const connectCeloWallet = async function () {
  if (window.celo) {
    try {
      notification("⚠️ Please approve this DApp to use it.")
      await window.celo.enable()
      notificationOff()
      const web3 = new Web3(window.celo)
      kit = newKitFromWeb3(web3)

      const accounts = await kit.web3.eth.getAccounts()
      kit.defaultAccount = accounts[0]

      contract = new kit.web3.eth.Contract(marketplaceAbi, MPContractAddress)
    } catch (error) {
      notification(`⚠️ ${error}.`)
    }
  } else {
    notification("⚠️ Please install the CeloExtensionWallet.")
  }
}

```

You assign your global `contract` variable a new `kit.web3.eth.Contract` object. You give it the ABI and address of your contract and it will convert your function calls into RPC. Now you can interact with your smart contract as if it were a Javascript object.

Now that you have an instance of the contract that you can interact with, you can call its functions. First, you want to display the products.

```
const getProducts = async function() {
  const _productsLength = await contract.methods.getProductsLength().call()
  const _products = []

```

You create a new `getProducts` function under your `getBalance` function.
First, you need to know how many products are stored in the contract so you can iterate over them. 
You create `_productsLength` to store the number of products and use `contract.methods` to call `getProductsLength()` from your contract and assign its value.

Next, you declare an empty array for the product’s objects.
```
  for (let i = 0; i < _productsLength; i++) {
    let _product = new Promise(async (resolve, reject) => {
      let p = await contract.methods.readProduct(i).call()
      resolve({
        index: i,
        owner: p[0],
        name: p[1],
        image: p[2],
        description: p[3],
        location: p[4],
        price: new BigNumber(p[5]),
        sold: p[6],
      })
    })
    _products.push(_product)
  }
  products = await Promise.all(_products)
  renderProducts()
}
```

Now you loop over the length of your products and for each product you create a promise, in this promise you call your contracts `readProduct`, to get the product data.
You resolve the promise with your product data and then push your `_product` object to your `_products` array. Be aware that the price needs to be a bigNumber object so you can later make correct payments.

When all promises of this asynchronous operation are fulfilled you render your products with the `renderProducts()` function that you created earlier.

Finally, you are going to enable the user to create a new product and save it to your contract.

```
document
  .querySelector("#newProductBtn")
  .addEventListener("click", async (e) => {
    const params = [
      document.getElementById("newProductName").value,
      document.getElementById("newImgUrl").value,
      document.getElementById("newProductDescription").value,
      document.getElementById("newLocation").value,
      new BigNumber(document.getElementById("newPrice").value)
      .shiftedBy(ERC20_DECIMALS)
      .toString()
    ]
    notification(`⌛ Adding "${params[0]}"...`)
```

You adapt your event handler for the "click" event on the `newProductBtn` Button.
In the params array, you store all the parameters that you read out from the form.
Again when somebody inputs a price value, you need to create a bigNumber and convert it in a way that the contract can understand.

You then show a notification that you are in the process of adding a new product.

```
    try {
      const result = await contract.methods
        .writeProduct(...params)
        .send({ from: kit.defaultAccount })
    } catch (error) {
      notification(`⚠️ ${error}.`)
    }
    notification(`🎉 You successfully added "${params[0]}".`)
    getProducts()
  })
```

Again you use one of your contract methods, this time `writeProduct` with the parameters that you saved earlier. Since you now want to send a transaction to the smart contract and execute its method you also need the `send` method. 
As a parameter, you need to specify the value of the property `from` ,which is the address of the sender and in your case you can access the address of the user via `kit.defaultAccount`.

If you get an error you display it to the user, if the product is added you display that too and call your `getProducts()` function to show the new product.

You also need to change one line in your product template function.

```
function productTemplate(_product) {
  return `
    <div class="card mb-4">
      <img class="card-img-top" src="${_product.image}" alt="...">
      <div class="position-absolute top-0 end-0 bg-warning mt-4 px-2 py-1 rounded-start">
        ${_product.sold} Sold
      </div>
      <div class="card-body text-left p-4 position-relative">
        <div class="translate-middle-y position-absolute top-0">
        ${identiconTemplate(_product.owner)}
        </div>
        <h2 class="card-title fs-4 fw-bold mt-2">${_product.name}</h2>
        <p class="card-text mb-4" style="min-height: 82px">
          ${_product.description}             
        </p>
        <p class="card-text mt-4">
          <i class="bi bi-geo-alt-fill"></i>
          <span>${_product.location}</span>
        </p>
        <div class="d-grid gap-2">
          <a class="btn btn-lg btn-outline-dark buyBtn fs-6 p-3" id=${
            _product.index
          }>
            Buy for ${_product.price.shiftedBy(-ERC20_DECIMALS).toFixed(2)} cUSD
          </a>
        </div>
      </div>
    </div>
  `
}
```

Since you are dealing with BigNumbers you need to convert the price again.

Now call your `getProducts` function when the DApp is loaded.

```
window.addEventListener('load', async () => {
  notification("⌛ Loading...")
  await connectCeloWallet()
  await getBalance()
  await getProducts()
  notificationOff()
});
```

Ok, that’s it for this part, you now should be able to save new products via your frontend into your contract on the Celo Blockchain.

![](https://cdn.kapwing.com/final_60a241593cf34b007b5540c8_666200.gif)

Javascript Code
[Link to code](https://github.com/moritzfelipe/celo-development-101-data/blob/main/code/3-building-the-frontend-of-the-dapp/3-5-read-and-write-to-your-contract/index.js)

## 3.6 Pay function (8 min)
In this part of the tutorial, you will learn how to create the payment functionality. 

The user will pay with the Celo stablecoin cUSD, an ERC-20 token. “The ERC20 token standard is a standard API for tokens within smart contracts'' ([Celo Docs](https://docs.celo.org/developer-guide/celo-for-eth-devs)).

In order to interact with the ERC-20 interface, we are going to need its ABI.

Get the ABI from Remix and paste it into the erc20.abi.json file in your project.

[GIF] // Showing how to copy the ABI from Remix to the project.
```
import Web3 from "web3"
import { newKitFromWeb3 } from "@celo/contractkit"
import BigNumber from "bignumber.js"
import marketplaceAbi from "../contract/marketplace.abi.json"
import erc20Abi from "../contract/erc20.abi.json"
```

Now you need to import the ABI, as you have done before.

As you have seen you need the ABI and the address of a contract in order to interact with it.

We will provide you with the address of the cUSD contract on the alfajores testnet ([See cUSD on blockchain explorer](https://alfajores-blockscout.celo-testnet.org/address/0x874069fa1eb16d44d622f2e0ca25eea172369bc1/transactions)).

```
const ERC20_DECIMALS = 18
const MPContractAddress = "0x178134c92EC973F34dD0dd762284b852B211CFC8"
const cUSDContractAddress = "0x874069Fa1Eb16D44d622F2e0Ca25eeA172369bC1"

```

You create a global variable for the cUSD contract address.

Before you can make a transaction you first have to get the user's approval to make a transaction for a certain amount of token, the allowance.

```
async function approve(_price) {
  const cUSDContract = new kit.web3.eth.Contract(erc20Abi, cUSDContractAddress)

  const result = await cUSDContract.methods
    .approve(MPContractAddress, _price)
    .send({ from: kit.defaultAccount })
  return result
}
```

You need to create a new function for that, you can create it under the `connectCeloWallet` function and call it `approve`. It takes the price of a product as a parameter.

First, you create a cUSD contract instance with the ABI and the contract address, `cUSDContract`.

Now you can call the cUSD contract method approve. You need to specify the contract address that will be allowed to make transactions and the amount that it will be allowed to spend, the price of the product.
Again you also need to specify who is going to spend the cUSD token, in this case, that’s again the address stored in `kit.defaultAccount`.

You return the result for error handling. 

For the final part of your DApp, you want to enable the user to buy a product. Adapt your buy button event listener for that.

```
document.querySelector("#marketplace").addEventListener("click", async (e) => {
  if (e.target.className.includes("buyBtn")) {
    const index = e.target.id
    notification("⌛ Waiting for payment approval...")
    try {
      await approve(products[index].price)
    } catch (error) {
      notification(`⚠️ ${error}.`)
    }
```
When the user clicks on a buy button, you get the product id and show them a notification about what is happening. Next, you try to get the approval for the token amount that your contract wants to spend through the approval function that you just created.
```
    notification(`⌛ Awaiting payment for "${products[index].name}"...`)
    try {
      const result = await contract.methods
        .buyProduct(index)
        .send({ from: kit.defaultAccount })
      notification(`🎉 You successfully bought "${products[index].name}".`)
      getProducts()
      getBalance()
    } catch (error) {
      notification(`⚠️ ${error}.`)
    }
  }
})
```

Now that your contract has the approval to spend the amount of cUSD of the product price, you use the contract's `buyProduct` function. You need to specify the product index and the users’ address.

You display notifications and if there is no error you call your `getProducts` function again to show the updated products in your DApp, and `getBalance` to show the updated balance after the user bought the product.
  
That’s it! You created your first DApp on the Celo Blockchain. Congratulations 🎉.

To test this DApp properly you should create two accounts so that you can see how the first account earns cUSD when you buy a product with a second account.

Here is a gif showing you how your app should behave.
![](https://cdn.kapwing.com/final_60a24aa10243170052eb66e2_11465.gif)

Code for this part of the tutorial.
[Link to code](https://github.com/moritzfelipe/celo-development-101-data/blob/main/code/3-building-the-frontend-of-the-dapp/3-6-pay-function/index.js)

## 3.7 Hosting your DApp on GitHub pages (6 min)
In this very last part of the tutorial, we will show you how you can host your project on GitHub pages so other people can interact with it.

After you tested your DApp and everything behaves like it's supposed to behave you can build your DApp in the command-line interface with the command.

```
npm run build
```

Now inside a docs folder of your project, you should have an HTML and JS file.

1. Upload your project to a new GitHub repository.
2. Inside your repository click on settings and scroll down to a section called GitHub Pages.
3. As a source for your GitHub pages select the master branch and the docs folder.

It might take some minutes but then you should be able to visit your DApp under the URL that is displayed in the GitHub Pages section.

[GIF] // Gif showing process

That’s it! Congratulations, you are done with the tutorial and have built your first DApp 🎉.
Now apply your new knowledge and take part in the challenge of this course.
