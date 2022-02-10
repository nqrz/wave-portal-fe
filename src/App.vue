<script setup>
import { onMounted, ref } from "vue"
import { ethers } from "ethers"
import abi from "./utils/WavePortal.json"
import Waves from "./components/Waves.vue";

const currentAccount = ref('')
const allWaves = ref([])
const message = ref('')

const contractAddress = "0xd010c650A53080244c7812d5bAAA0ED45D58D1C7"

const contractABI = abi.abi;

const getAllWaves = async () => {
  try {
    const { ethereum } = window;
    if (ethereum) {
      const provider = new ethers.providers.Web3Provider(ethereum);
      const signer = provider.getSigner();
      const wavePortalContract = new ethers.Contract(contractAddress, contractABI, signer);

      const waves = await wavePortalContract.getAllWaves()

      let wavesCleaned = [];
      waves.forEach(wave => {
        wavesCleaned.push({
          address: wave.waver,
          timestamp: new Date(wave.timestamp * 1000),
          message: wave.message
        });
      });

      allWaves.value = wavesCleaned
    } else {
      console.log("Ethereum object doesn't exist!")
    }
  } catch (error) {
    console.log(error);
  }
}

const checkIfWalletIsConnected = async () => {
  try {
    const { ethereum } = window;

    if (!ethereum) {
      console.log("Make sure you have metamask!");
    } else {
      console.log("We have the ethereum object", ethereum);
    }

    const accounts = await ethereum.request({ method: "eth_accounts"});

    if (accounts.length !== 0) {
      const account = accounts[0];
      console.log("found an authorized account:", account);
      currentAccount.value = account
      getAllWaves()
    } else {
      console.log("No authorized account found")
    }
  } catch (error) {
    console.log(error);
  }
}

const connectWallet = async () => {
  try {
    const { ethereum } = window;

    if (!ethereum) {
      alert("Get MetaMask!");
      return;
    }

    const accounts = await ethereum.request({ method: "eth_requestAccounts" });

    console.log("Connected", accounts[0]);
    currentAccount.value = accounts[0]
  } catch (error) {
    console.log(error)
  }
}

const handleWave = async () => {
  try {
    const { ethereum } = window;

    if (ethereum) {
      const provider = new ethers.providers.Web3Provider(ethereum);
      const signer = provider.getSigner();
      const wavePortalContract = new ethers.Contract(contractAddress, contractABI, signer);

      let count = await wavePortalContract.getTotalWaves();
      console.log("Retrieved total wave count...", count.toNumber());

      const waveTxn = await wavePortalContract.wave(message.value);
      console.log("Mining...", waveTxn.hash);
      message.value = ''

      await waveTxn.wait();
      console.log("Mined -- ", waveTxn.hash);

      count = await wavePortalContract.getTotalWaves();
      console.log("Retrieved total wave count..", count.toNumber());
    } else {
      console.log("Ethereum object doesn't exist!");
    }
  } catch (error) {
    console.log(error);
  }
}

onMounted(() => {
  checkIfWalletIsConnected()
})
</script>

<template>
  <a class="tooltip link" href="http://buildspace.so" target="_blank" rel="noopener noreferrer">
    <button class="btn">
      🦄 Buildspace
    </button>
  </a>
  <div class="container bg-banner" style="height: 100vh;">
    <figure>
      <img class="image" src="./assets/ijay.jpg" alt="Nizar Baihaqi photo">
    </figure>
    <h1>
      <a title="Go to my website" class="link" href="http://nizarbaihaqi.com" target="_blank" rel="noopener noreferrer">
        Nizar Baihaqi &#8599;
      </a>
    </h1>
    <p style="padding: 14px;">
      Hello folks, I'm Nizar Baihaqi. A Fullstack web dev that always confused which framework to use. 
    </p>
    <p>
      Wave at me and share your thoughts
    </p>
    <div v-if="!currentAccount" class="form-container">
      <button @click="connectWallet" class="btn" style="width: 100%;">
        Connect Wallet
      </button>
      <h6 style="color: gray;">You have to connect your wallet to message me.</h6>
    </div>
    <form v-else @submit.prevent="handleWave" class="form-container">
      <input class="input" v-model="message" type="text" name="message" id="message" placeholder="Message here">
      <button class="btn" style="align-self: end;" type="submit">
        Wave 👋
      </button>
    </form>
    <div>
      {{ message }}
    </div>
  </div>
  <Waves :waves="allWaves" />
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans:ital,wght@0,400;0,700;1,400;1,700&display=swap');

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Noto Sans', sans-serif;
  margin: 0;
  padding: 0;
}

.tooltip {
  position: fixed;
  top: 0;
  right: 0;
  display: flex;
  gap: 10px;
  margin-right: 10px;
}

.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.bg-banner {
  background-image: linear-gradient(to bottom left, whitesmoke, lightblue);
}

figure {
  background-image: linear-gradient(to top right, blue, gray, red, yellow);
  width: 150px;
  height: 150px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.image {
  max-width: 140px;
  border-radius: 50%;
}

.link {
  text-decoration: none;
}

.link:hover {
  text-decoration: underline;
}

.form-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 345px;
}

.input {
  box-sizing: border-box;
  padding: 10px;
  border-radius: 4px;
  border: none;
  width: 100%;
}

.btn {
  border: none;
  margin-top: 10px;
  padding: 10px;
  padding-top: 8px;
  padding-bottom: 8px;
  border-radius: 4px;
}
</style>
