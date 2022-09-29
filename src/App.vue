<script>
  import axios from 'axios';

  export default {
    data() {
      return {
        baseApiURL: 'http://localhost:4000/api',
        CryptosList: {},
        CryptoName: '',
        CryptoArray: []
      }
    },
    created() {
      this.getCryptos()
    },
    methods: {
      getCryptos() {
        axios.get(this.baseApiURL).then(res => {
         
          let singleName = this.CryptoName
          this.CryptoArray.push(singleName)
          let names = this.CryptoArray.join();

          if (!names) {
            this.CryptosList = {}
          } else {
              axios.get('https://min-api.cryptocompare.com/data/pricemulti?fsyms=' + names + '&tsyms=USD&api_key=569290af3b7471de24fde18f50d87b7f39a8c5a0ad4e11c56669a182b70b4fd3').then(res => {
              this.CryptosList = res.data;
              this.$refs.CryptoInput.reset();
            }).catch(error => {
              console.log(error)
            })
            this.CryptoName = ''
          }

        })
      },
      handleSubmitForm() {
        axios.post(this.baseApiURL + '/add-crypto', this.crypto).then(() => {
          this.getCryptos()
        }).catch(error => {
          console.log(error)
        })
      }
    }
  }
</script>

<template>
  <div>
    <div class="container">
      <a href="#"><img class="logo" src="./assets/logo.png" alt="Logo"></a>

      <form @submit.prevent="handleSubmitForm" ref='CryptoInput'>
        <div class="form-group">
          <label for="crypto">Crypto:</label>

          <div class="form-group-inner">
            <input type="text" class="crypto" v-model="CryptoName" placeholder='BTC' required>
            <button class="add">Add</button>
          </div>
          <p class="info">Use Abbreviations</p>

        </div>
      </form>

      <div class="crypto-container">
        <div class="crypts" v-for='(item, name) in CryptosList' :key='item._id'>
          <p class="price">${{ item.USD.toLocaleString() }}</p>
          <div class="lower">
            <p class="crypto-label">{{ name }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Quando&display=swap');

  * {
    box-sizing: border-box;
  }

  body{
    background: #2E4052;
    color: #A0CED9;
    margin: 0;
    height: 100vh;
    font-family: 'Quando', serif;
  }

  .logo {
    width: 10em;
    margin: 3em 0 0;
  }

  .container {
    text-align: center;
    margin: 0 1em;
    background: #2E4052;
    color: #A0CED9;
    margin: 0;
    height: 100vh;
    font-family: 'Quando', serif;
  }

  .form-group {
    text-align: left;
    width: 300px;
    margin: 7em auto;
  }

  label {
    margin-bottom: .7em;
    display: block;
  }

  input {
    padding: 1em;
    outline: none;
    border: none;
    border-top-left-radius: .3em;
    border-bottom-left-radius: .3em;
    font-size: 1.3em;
  }

  .form-group-inner {
    display: grid;
    grid-template-columns: 100px auto;
  }

  button.add {
    font-size: 1em;
    color: white;
    background-color: #EF7B45;
    border: none;
    outline: none;
    padding: 1em 3em;
    border-top-right-radius: .3em;
    border-bottom-right-radius: .3em;
    cursor: pointer;
  }

  p.info {
    color: #A0CED9
  }

  .crypto-container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(340px, 1fr));
    grid-gap: 1em;
    padding-bottom: 3em;
    padding-right: 1.5em;
    padding-left: 1.5em;
  }

  .crypts {
    background: #395066;
    border-top-right-radius: 5px;
    border-top-left-radius: 5px;
    border-bottom-left-radius: 5px;
    border-bottom-right-radius: 5px;
  }

  .price {
    font-size: 3em;
    padding: 0 1em;
  }

  .lower {
    background: #354f67;
    padding: 1em;
    position: relative;
    height: 100px;
    border-bottom-right-radius: 5px;
    border-bottom-left-radius: 5px;
  }

  .crypto-label {
    text-transform: uppercase;
    font-size: 1.5em;
    color: #EF7B45;
  }

  .delete-btn {
    background: none;
    border: none;
    position: absolute;
    bottom: 1em;
    right: 1em;
    cursor: pointer;
    outline: none;
  }

</style>
