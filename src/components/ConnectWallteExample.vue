<template>
  <div>
    <button @click="handleWalletConnect">WalletConnect</button>
    <button @click="resetApp">resetApp</button>
    <button @click="getAccountAssets">getBalance</button>
    <button @click="approve">approveUSDTContract</button>
    <p>
      Address:
      {{ userAddress }}
    </p>
    <p>balance:{{ assets }}</p>
    <p>networkId: {{ networkId }}</p>
    <p>chainId: {{ chainId }}</p>
  </div>
</template>
<script setup>
import { computed } from 'vue';
import { utils } from 'web3';
import useWallet from '../hooks/useWallte';
import {USDT_ABI} from '../web3/abis';
import { USDT_ADDRESS } from '../web3/config';

const {
  onConnect,
  connected,
  web3,
  userAddress,
  chainId,
  networkId,
  resetApp,
  assets,
  getAccountAssets,
} = useWallet();

const handleWalletConnect = async () => {
  await onConnect();
  if (connected) {
    console.log('afterConnectdWallet', connected);
  }
};
const contract = computed(
  () => new web3.value.eth.Contract(USDT_ABI, USDT_ADDRESS),
);

function approve() {
  return contract.value.methods
    .approve('0x80b3442d58335C0E0F920c7827086c09Ffe7a55f', utils.toHex(utils.toWei('1000000', 'gwei')))
    .send({ from: userAddress.value });
}




</script>
