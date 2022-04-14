# erc20Wallet
## Introduction
It is a package to be able to work with tokens and ethereum, to send and receive.

Facility
Install from npm:

## npm i erc20-wallet -s
Then we need to import the package into our project

let erc20 = require( ' erc20-wallet ' ) ;
Once we import it now if we can start working with it.

## First time startup configuration
1.- The first thing we must do is create the 12-word seed. In order to create it, we must enter a password and an alphanumeric word to encrypt the seed with those parameters. That seed the user must save it since with it he restores the wallet at any time. First we create the password and the alphanumeric word as follows:

erc20 . password  =  'mypassword' ; 
erc20 . mySeed  =  'myalphanumericword8989' ;
After entering my password and my alphanumeric word, we continue to create the seed as follows:

     await erc20 . createSeed ( ) . then ( ( response )  =>  { 
        erc20 . seed  =  response ; 
    } ) . catch ( ( error )  =>  { 
        console . error ( error ) ; 
    } ) ;
Note: I recommend you save the seed in the erc20.seed variable since we will need it to create the keystore or the wallet later on.

Now we have to create the keystore or the wallet, the first time we create it we must save it in a file locally so that later we only send it to call and do not create the seed or a new wallet. It is created as follows:

     await erc20 . createdStored ( ) . then ( ( response )  =>  { 
        erc20 . keystore  =  response ; 
    } ) . catch ( ( error )  =>  { 
        console . error ( error ) ; 
    } ) ;
We must store the response in the erc20.keystore variable since we will call it later.

Now we have to create the ethereum addresses, here we must define the number of wallets to create and then call the method to create them, to configure the amount is as follows:

erc20 . numAddr  =  10 ;

 await erc20 . generateAddress ( ) . then ( ( response )  =>  { 
     erc20 . address  =  response ; 
} ) . catch ( ( error )  =>  { 
     console . error ( error ) ; 
} ) ;
This will create 10 wallets and return a json as follows:

[  
  {  address : '0xxxxxxxxxxxxxxxxxxxxxxxxxxx'  } , 
  {  address : '0xxxxxxxxxxxxxxxxxxxxxxxxxx'  } , 
  {  address : '0xxxxxxxxxxxxxxxxxxxxxxxxxxx'  } 
]
Once this process is finished, we continue with the step where the seed was already created, the wallets and addresses were created, now we only need to save the keystore in a local file, since we will send that file to call every time we need to do something , these previous steps, are only the first time that everything is configured. The following is with the keystore saved in the file.

To save the keystore in a file, we must first call it to create a json stringify, we send it to create as follows:

     await erc20 . encodeJson ( ) . then ( ( response )  =>  { 
        console . log ( response ) ; 
    } ) . catch ( ( error )  =>  { 
        console . error ( error ) ; 
    } ) ;
We send the answer to save in a file.

Now to decode it and configure it for the other actions is as follows:

    erc20 . keystoreJson  =  'The content of the file we saved goes here' ; 
    await erc20  . decodeJson ( ) . then ( ( response ) => { erc20 . keystore = response ; } ) . catch ( ( error ) => { console . error ( error ) ; } ) ;  
          
      
        
    
Where it says erc20.keystoreJson goes the content that we save in the file. We must store the response in erc20.keystore , since it is where the keystore is stored to be called to send transactions.

## Token Configuration
We must configure the address of the smart contract, as well as the decimals, the symbol and the name, it is done as follows:

    erc20 . tokenAddr  =  'Eth address of the token' ; 
    erc20 . tokenDecimals  =  18 ;  //the decimal places of the 
    erc20 token . tokenSymbol  =  '' ;  //The symbol of the 
    erc20 token . tokenName  =  '' ;  // The name of the token
If you only know the address, set the address of the token, and there is a method that returns the decimals, the symbol and the name of the token, it is called as follows:

    erc20 . tokenAddr  =  'Eth address of the token' ; 
    await erc20  . getDataToken ( ) . then ( ( response ) => { erc20 . tokenSymbol = response . symbol ; erc20 . tokenName = response . name ; erc20 . tokenDecimals = response . decimals ; } ) . catch ( (  
          
          
          
    error )  =>  { 
        console . error ( error ) ; 
    } ) ;
The method returns the info and saves them in their respective variables.

## Balance eth and tokens
To see balances, first configure the provider, that is, the network or a geth node, in this case I recommend using infura , it is configured as follows:

    erc20 . provider  =  'https://mainet.infura.io/v3/projectId' ;
In this case I am using the ropsten test network, register in infura, create a project and configure it, in the case that it is mainet, put the mainet provider.

To be able to see the eth balance of an address, do the following:

     await erc20 . getBalanceAddress ( 'address' ) . then ( ( response )  =>  { 
        console . log ( response ) ; 
    } ) . catch ( ( error )  =>  { 
        console . error ( error ) ; 
    } ) ;
Where it says address there goes the ethereum address, it will return the amount of balance in eth.

To be able to see the balance of tokens of an address, it is done as follows:

     await erc20 . getTokenAddress ( 'address' ) . then ( ( response )  =>  { 
        console . log ( response ) ; 
    } ) . catch ( ( error )  =>  { 
        console . error ( error ) ; 
    } ) ;
Where it says address, there goes the ethereum address, it will return the amount of token balance.

## Ethereum Shipping
You must first configure the provider, seen in the previous step.

Before sending ethereum I recommend you calculate the gaslimit and gasprice, this is used to calculate what commission the ethereum network will charge us, we must send our address from which we will send, also the destination address and the amount of eth to send.

     await erc20 . calculateGasLimitEth ( 'LocalAddress' ,  'DestinationAddress' ,  amount ) . then ( ( response )  =>  { 
        console . log ( response ) ; 
    } ) . catch ( ( error )  =>  { 
        console . error ( error ) ; 
    } ) ;
This will return a json with the gasLimit and gasPrice data that we must send when sending the transaction, these data are calculated from the current network. The json response is as follows:

{  gasLimit : 21000 ,  gasPrice : 1050000000  }
This is an answer that gave us back the calculation.

Now to send a transaction we must enter our password with which we create our wallet, my source address, destination address, amount, gasPrice and gasLimit, to send it is done as follows:

     await erc20 . sendETH ( 'password' ,  ' LocalAddress' , 'RecipientAddress  ' ,  amount ,  gasPrice ,  gasLimit ) . then ( ( response )  =>  { 
        console . log ( response ) ; 
    } ) . catch ( ( error )  =>  { 
        console . error ( error ) ; 
    } ) ;
This will return the hash if the transaction was successful.

## Token Sending
You must first configure the provider, seen in the previous step.

Before sending tokens I recommend you calculate the gaslimit and gasprice, this is used to calculate what commission the ethereum network will charge us, we must send our address from which we will send, also the destination address and the amount of eth to send.

     await erc20 . calculateGasLimitToken ( 'LocalAddress' ,  'DestinationAddress' ,  amount ) . then ( ( response )  =>  { 
        console . log ( response ) ; 
    } ) . catch ( ( error )  =>  { 
        console . error ( error ) ; 
    } ) ;
This will return a json with the gasLimit and gasPrice data that we must send when sending the transaction, these data are calculated from the current network. The json response is as follows:

{  gasLimit : 40308 ,  gasPrice : 1050000000  }
This is an answer that gave us back the calculation.

Now to send a transaction we must enter our password with which we create our wallet, my source address, destination address, amount, gasPrice and gasLimit, to send it is done as follows:

     await erc20 . sendTokens ( 'password' ,  ' LocalAddress' , 'RecipientAddress  ' ,  quantity ,  gasPrice ,  gasLimit ) . then ( ( response )  =>  { 
        console . log ( response ) ; 
    } ) . catch ( ( error )  =>  { 
        console . error ( error ) ; 
    } );
This will return the hash if the transaction was successful.

## Configure etherscan api for transaction listing
First we must create an account in etherscan.io and create an api, or alternatively leave the default, what we must configure is the network, say if it is ropsten or mainet and the timeout, it is done as follows:

    erc20 . apikeyEtherScan  =  'YourApiKey' ; 
    erc20 . networkEtherScan  =  'ropsten' ; 
    erc20 . timeoutScan  =  3000 ;
The networks it supports are: morden //This is the mainet ropsten rinkeby

## List of ETH transactions
This is to return the list of transactions sent and received from an address. The call is made as follows:

In DireccionLocal goes my address.

     await erc20 . getTxtEth ( 'LocalAddress' ) . then ( ( response )  =>  { 
        console . log ( response ) ; 
    } ) . catch ( ( error )  =>  { 
        console . error ( error ) ; 
    } ) ;
This will return a json as follows:

[  {  Type : 'Sent' , 
    BlockNumber : '7327297' , 
    timeStamp : '1581701462' , 
    hash :
      '0xeb144420c9edba10281646b357877ac0debeec5ecdde6174b87175ec8f746b07' , 
    nonce : 18 , 
    from : '0xd353a3fd2a91dbc1faea041b0d1901a7a0978434' , 
    to : '0xf3edd3916edcc0ee8f76ad0d33f2bcf0c424d899' , 
    value : 0001 , 
    gas : 21630 , 
    gasPrice : 1.197971241e-9 ,
    isError : 0 , 
    confirmations : 62  } , 
  {  type : 'Received' , 
    BlockNumber : '7270076' , 
    timeStamp : '1580945739' , 
    hash :
      '0xa2d4969333699440f54a1e7fc634cb19ccee5b9baf5d8a246fc3e8b82d79d706' , 
    nonce : 29486096 , 
    from : '0x81b7e08f65bdf5648606c89998a9cc8164397647' , 
    to : '0xd353a3fd2a91dbc1faea041b0d1901a7a0978434' , 
    value : 1 ,
    gas : 21000 , 
    gasPrice : 2e-9 , 
    isError : 0 , 
    confirmations : 57,283  } , 
  {  type : 'Received' , 
    BlockNumber : '7270075' , 
    timeStamp : '1580945723' , 
    hash :
      '0xbef98dca4a169799b813f26a95b9db040cd7a842915959a29114b3ca4cfe6a7d' , 
    nonce : 29486094 , 
    from : '0x81b7e08f65bdf5648606c89998a9cc8164397647' , 
    to :'0xd353a3fd2a91dbc1faea041b0d1901a7a0978434' , 
    value : 1 , 
    gas : 21000 , 
    gasPrice : 2e-9 , 
    isError : 0 , 
    confirmations : 57,284  } , 
  {  type : 'Received' , 
    BlockNumber : '7270073' , 
    timeStamp : '1580945690' , 
    hash :
      '0xca72913e032b0dc96da8861470d83733f10c8b0e58a8a202ebe0a76e06d86314 ' , 
    nonce : 29486092 ,
    from : '0x81b7e08f65bdf5648606c89998a9cc8164397647' , 
    to : '0xd353a3fd2a91dbc1faea041b0d1901a7a0978434' , 
    value : 1 , 
    gas : 21000 , 
    gasPrice : 2e-9 , 
    isError : 0 , 
    confirmations : 57,286  } , 
  {  type : 'Received' , 
    BlockNumber : '7269361' , 
    timeStamp : '1580936345' , 
    hash :
     '0x0a43818a1d4a8e1ae5049c09d2ef854225ec4829b012f545dcfc05a05321d279' , 
    nonce : 41 , 
    from : '0x8ab9af40b0e5ef87a8ab94149ff9cb06d5fcd599' , 
    to : '0xd353a3fd2a91dbc1faea041b0d1901a7a0978434' , 
    value : 0.05 , 
    gas : 21000 , 
    gasPrice : 6e-9 , 
    isError : 0 , 
    confirmations : 57998  }  ]
This is an example, isError if it is 1 it is an erroneous transaction, if it is 0 it is a successful transaction.

## List of Token transactions
This is to return the list of transactions sent and received from an address. The call is made as follows:

In DireccionLocal goes my address.

     await erc20 . getTxtTokens ( 'LocalAddress' ) . then ( ( response )  =>  { 
        console . log ( response ) ; 
    } ) . catch ( ( error )  =>  { 
        console . error ( error ) ; 
    } ) ;
This will return a json as follows:

[  {  Type : 'Sent' , 
    BlockNumber : '7310213' , 
    timeStamp : '1581481211' , 
    hash :
      '0x50c9c168a02ff2a3855650f8941f6b01280bdb0e831569a78f7d505350704148' , 
    nonce : 17 , 
    from : '0xd353a3fd2a91dbc1faea041b0d1901a7a0978434' , 
    to : '0x5bc6452d23386effed5651912b3fdac2f4595f67' , 
    value : 1 , 
    gas : 57583 , 
    gasPrice : 5.5e-9 , 
    isError: 0 , 
    confirmations : 17,173  } , 
  {  type : 'Received' , 
    BlockNumber : '7269372' , 
    timeStamp : '1580936482' , 
    hash :
      '0x598da68aabb7837c766805f722ae1956eed70ab936de8faf6765fd991ff7517f' , 
    nonce : 42 , 
    from : '0x8ab9af40b0e5ef87a8ab94149ff9cb06d5fcd599' , 
    to : '0xd353a3fd2a91dbc1faea041b0d1901a7a0978434' , 
    value : 15 , 
    gas :80083 , 
    gasPrice : 8e-9 , 
    isError : 0 , 
    confirmations : 58014  }  ]
This is an example, isError if it is 1 it is an erroneous transaction, if it is 0 it is a successful transaction.

Recover Wallet
We must send the 12 keywords that we keep from our seed, and create a password

erc20 . seed  =  'MY 12 words' ; 
erc20 . password  =  'A password' ;
After we create the keystore, and once that is done, the rest of creating the wallets and others is the same as the previous steps

     await erc20 . createdStored ( ) . then ( ( response )  =>  { 
        erc20 . keystore  =  response ; 
    } ) . catch ( ( error )  =>  { 
        console . error ( error ) ; 
    } ) ;
## Errors and contributions
For a bug, write the problem directly on github issues or send it to info@ouchestechnology.com.ng . If you want to contribute to the project please send an email.

#OuchesTech , #OuchesTechnology , #ethereum , #tokens , #wallets , #erc20
