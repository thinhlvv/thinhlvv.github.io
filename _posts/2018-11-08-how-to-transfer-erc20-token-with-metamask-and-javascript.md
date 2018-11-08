---
layout: post
published: true
title: How to transfer ERC20 token with Metamask and Javascript.
---

### Problem
I got trouble in defining a contractInstance with web3. It didn’t list functions of contract out. Spent one day to take around the tutorials and got the same solution is must define contract.

### How to solve it?
Luckily for my boss when I saw the [warning of Metamask](https://medium.com/metamask/https-medium-com-metamask-breaking-change-injecting-web3-7722797916a8) and see the updates, my life was saved at that moment.

This is simple code that works with me after one day thrown away in the same articles.

- Change your metamask to ropsten network.
- Write this to fresh index.html

```
<script src="https://cdn.jsdelivr.net/gh/ethereum/web3.js/dist/web3.min.js"></script>
    <script type="text/javascript">
        window.addEventListener('load', async() => {
            if (window.ethereum){
                window.web3 = new Web3(ethereum);
                try {
                    await ethereum.enable();
                } catch(error) {
                    document.getElementById('metamask').innerHtml = "Error occured when opening Metamask"+ error
                }
            }else if (window.web3){
                window.web3 = new Web3(web3.currentProvider);
            }else{
                document.getElementById('metamask').innerHtml = 'Please download and install Metamask: <a href="https://metamask.io/">https://metamask.io/</a>'
            }
        });

        function openMetamask(){
                var contractAddress = {{.ContractAddress}}
                var receiver = {{.PALFoundationAddress}}
                var currentUser = web3.eth.accounts[0]
                var ethAmount = 0
                var contractABI = [{"constant":true,"inputs":[],"name":"name","outputs":[{"name":"","type":"string"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"_spender","type":"address"},{"name":"_value","type":"uint256"}],"name":"approve","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"totalSupply","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"_from","type":"address"},{"name":"_to","type":"address"},{"name":"_value","type":"uint256"}],"name":"transferFrom","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"decimals","outputs":[{"name":"","type":"uint8"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"_value","type":"uint256"}],"name":"burn","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"tokenSaleContract","outputs":[{"name":"","type":"address"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"_spender","type":"address"},{"name":"_subtractedValue","type":"uint256"}],"name":"decreaseApproval","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"name":"_owner","type":"address"}],"name":"balanceOf","outputs":[{"name":"balance","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"owner","outputs":[{"name":"","type":"address"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"isTokenTransferable","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"symbol","outputs":[{"name":"","type":"string"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"_to","type":"address"},{"name":"_value","type":"uint256"}],"name":"transfer","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[{"name":"_spender","type":"address"},{"name":"_addedValue","type":"uint256"}],"name":"increaseApproval","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[{"name":"_toggle","type":"bool"}],"name":"toggleTransferable","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[{"name":"_token","type":"address"},{"name":"_amount","type":"uint256"}],"name":"emergencyERC20Drain","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"name":"_owner","type":"address"},{"name":"_spender","type":"address"}],"name":"allowance","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"newOwner","type":"address"}],"name":"transferOwnership","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"inputs":[{"name":"_tokenTotalAmount","type":"uint256"},{"name":"_adminAddr","type":"address"}],"payable":false,"stateMutability":"nonpayable","type":"constructor"},{"anonymous":false,"inputs":[{"indexed":true,"name":"previousOwner","type":"address"},{"indexed":true,"name":"newOwner","type":"address"}],"name":"OwnershipTransferred","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"name":"burner","type":"address"},{"indexed":false,"name":"value","type":"uint256"}],"name":"Burn","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"name":"owner","type":"address"},{"indexed":true,"name":"spender","type":"address"},{"indexed":false,"name":"value","type":"uint256"}],"name":"Approval","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"name":"from","type":"address"},{"indexed":true,"name":"to","type":"address"},{"indexed":false,"name":"value","type":"uint256"}],"name":"Transfer","type":"event"}]

                var contractInstance = web3.eth.contract(contractABI).at(contractAddress);
                contractInstance.balanceOf(currentUser,function(err, amount){
                    contractInstance.transfer(receiver, amount, {
                        from: currentUser,
                        to: contractAddress
                    }, function (error, result) {
                        if (error) {
                            document.getElementById('metamask').innerHTML = "Ok, transfer by yourself."
                        } else {
                            document.getElementById('metamask').innerHTML = "Track the payment: <a href='https://etherscan.io/tx/" + result + "'>https://etherscan.io/tx/" + result + "'"
                        }
                    });
                });
      }
    </script>

<div id="metamask">
	<button onclick="openMetamask()" type="button" class="btn btn-primary"> Open Metamask </button>
</div>

```

- Create a simple server and serve index.html, you can see how it works.

Yay, road to blockchain developer never be easy like this. 

Catch me up with bugs if you can.

*Note: gophers working with beego*

`bee new metamask`

`mv index.html ./metamask/views/index.tpl`

`cd metamask && bee run`
