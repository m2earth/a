<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Real Estate Token</title>
</head>
<body>
    <h1>Real Estate Token Dashboard</h1>
    <div>
        <p>Your Address: <span id="userAddress"></span></p>
        <p>Token Balance: <span id="tokenBalance"></span> RET</p>
        <button onclick="mintTokens()">Mint Tokens</button>
        <button onclick="burnTokens()">Burn Tokens</button>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/web3@1.3.4/dist/web3.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/@truffle/contract@4.3.11/dist/truffle-contract.min.js"></script>
    <script>
        const contractAddress = 'YOUR_CONTRACT_ADDRESS';
        const contractABI = []; // Add your contract ABI here

        window.addEventListener('load', async () => {
            if (window.ethereum) {
                window.web3 = new Web3(window.ethereum);
                await window.ethereum.enable();
                const accounts = await web3.eth.getAccounts();
                displayUserInfo(accounts[0]);
                displayTokenBalance(accounts[0]);
            } else {
                console.error('Web3 not detected. Please install MetaMask.');
            }
        });

        async function displayUserInfo(address) {
            document.getElementById('userAddress').innerText = address;
        }

        async function displayTokenBalance(address) {
            const contract = new web3.eth.Contract(contractABI, contractAddress);
            const balance = await contract.methods.balanceOf(address).call();
            document.getElementById('tokenBalance').innerText = balance;
        }

        async function mintTokens() {
            const amount = prompt('Enter the amount to mint:');
            const accounts = await web3.eth.getAccounts();
            const contract = new web3.eth.Contract(contractABI, contractAddress);
            await contract.methods.mint(accounts[0], amount).send({ from: accounts[0] });
            displayTokenBalance(accounts[0]);
        }

        async function burnTokens() {
            const amount = prompt('Enter the amount to burn:');
            const accounts = await web3.eth.getAccounts();
            const contract = new web3.eth.Contract(contractABI, contractAddress);
            await contract.methods.burn(amount).send({ from: accounts[0] });
            displayTokenBalance(accounts[0]);
        }
    </script>
</body>
</html>
