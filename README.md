# Gnoland Cheatsheet

Github : github.com/slashbinslashnoname/gnoland_cheatsheet

You need gnokey to make queries `./gnokey query` and transactions `./gnokey maketx`.
You can build and install gnokey following this tutorial : https://gnoland.space/start/create-wallet




## Installing Gnokey and Gnodev

Compiling and installing gnokey is *really* easy.


### Compiling
````
git clone git@github.com:gnolang/gno.git
cd ./gno
make
````

### Installing gnodev

``` make install_gnodev```


### Installing gnokey

``` make install_gnokey```


## Parameters for the next step
For testnet "test2"

I used to send transactions with these parameters.
* Min fee : 1ugnot
* Gas : 15000000 (reimbursed if not used)



## Account



### List accounts

Useful to get your address

```
./gnokey list
```

The address used in this tutorial is `g1ah79e3txw2kd2e8dscr2y2ucr888lm3qwm3v6e`. Change it by your own address :slightly_smiling_face: 


### Query account 

Useful to get the sequence of your transactions and your account number

```
./gnokey query "auth/accounts/g1ah79e3txw2kd2e8dscr2y2ucr888lm3qwm3v6e" --remote test2.gno.land:36657

height: 0
data: {
  "BaseAccount": {
    "address": "g1ah79e3txw2kd2e8dscr2y2ucr888lm3qwm3v6e",
    "coins": "489988991ugnot",
    "public_key": {
      "@type": "/tm.PubKeySecp256k1",
      "value": "Ap96kZn7jX9lTBIXj95YZLdTtm95hu57+VNVKCoECbFI"
    },
    "account_number": "660936",
    "sequence": "18"
  }
}
```



## Board


### Create a board

Change `args` and Account named `g1ah79e3txw2kd2e8dscr2y2ucr888lm3qwm3v6e` with data parsed in previous steps.

> Args
> Title : slashbin
> 
> Account
> Number : 660936
> Sequence : 18


```
*** Create Board Transaction

./gnokey maketx call g1ah79e3txw2kd2e8dscr2y2ucr888lm3qwm3v6e --pkgpath "gno.land/r/boards" --func CreateBoard --args "slashbin" --chainid "test2" --send 100000000ugnot --gas-fee 1ugnot --gas-wanted 4000000 > createboard.unsigned.txt

*** Sign transaction 

./gnokey sign g1ah79e3txw2kd2e8dscr2y2ucr888lm3qwm3v6e --txpath createboard.unsigned.txt --chainid "test2" --number 660936 --sequence 18 > createboard.signed.txt

*** Broadcast Transaction

./gnokey broadcast createboard.signed.txt --remote test2.gno.land:36657
```


Other solution with autosign : 

```

./gnokey maketx call g1ah79e3txw2kd2e8dscr2y2ucr888lm3qwm3v6e --pkgpath "gno.land/r/boards" --func CreateBoard --args "slashbin" --chainid "test2" --broadcast true --send 100000000ugnot --gas-fee 1ugnot --gas-wanted 4000000 --remote "test2.gno.land:36657"
```

## Create a Thread

You first need to get the Board ID

### Get the Board ID

Pushing a transaction (cost gas)
```
./gnokey maketx call g1ah79e3txw2kd2e8dscr2y2ucr888lm3qwm3v6e --pkgpath "gno.land/r/boards" --func "GetBoardIDFromName" --gas-fee 1ugnot --gas-wanted 4000000  --broadcast true --chainid test2 --args "slashbin" --remote test2.gno.land:36657
```

Making a simple query (doesn't cost gas)

> Notice the newline \n, used to pass multiple instructions.   
```
./gnokey query "vm/qeval" --data "gno.land/r/boards\nGetBoardIDFromName(\"slashbin\")" --remote test2.gno.land:36657
```

### Create a thread on the Board

Use the args gotten previously (`1461` in our example) 

> Title : *Hire me*
> Content : *https://twitter.com/slashbin_fr*


```
./gnokey maketx call g1ah79e3txw2kd2e8dscr2y2ucr888lm3qwm3v6e --pkgpath "gno.land/r/boards" --func "CreateThread" --gas-fee 1000ugnot --gas-wanted 4000000 --broadcast true --chainid test2 --args "1461" --args "Hire me" --args "https://twitter.com/slashbin_fr" --remote test2.gno.land:36657
```

### Render the Board

> Notice that we got rid of the newline \n, it works well too  

```
./gnokey query "vm/qrender" --data "gno.land/r/boards            
slashbin" --remote test2.gno.land:36657
```


### Create a reply

> Notice the higher gas fee. I used the next step to verify why the transaction was not validated.
```
./gnokey maketx call g1ah79e3txw2kd2e8dscr2y2ucr888lm3qwm3v6e --pkgpath "gno.land/r/boards" --func "CreateReply" --gas-fee 1ugnot --gas-wanted 5000000  --broadcast true --chainid test2 --args "1461" --args "1" --args "1" --args "https://github.com/slashbinslashnoname/gnoland_cheatsheet" --remote test2.gno.land:36657
```


## Verifying your transaction


You can verify your transaction by using rpc.gno.tools with the tx.height used to debug your transaction.

https://rpc.gno.tools/tx_search?query="tx.height=202977"


## Outro

Next tutorial will learn you how to deploy a very simple smartcontract.

I hope you enjoyed this tutorial.

You can reach me at twitter.com/slashbin_fr and you already have my Gnoland account :smiley_cat: :black_heart: 


