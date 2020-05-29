<template>
  <section>
    <h1>KEANet</h1>
    <div>
      <!--Internet Checkbox-->
      <table id="MainTable">
        <tr>
          <td width="20%">
            <input
              type="checkbox"
              id="chkInternetConnection"
              v-model="internet"
              @change="checkInternet"
            />
          </td>

          <td width="80%">
            <table>
              <label for="chkInternetConnection">Internet connection</label>
            </table>
          </td>
        </tr>
      </table>

      <!--Phone Lines-->
      <table id="MainTable">
        <tr>
          <td width="20%">
            <input type="number" id="txtPhoneLines"  v-model.number=numberOfLines size="1" min="0" max="8" @change="phoneLines">
          </td>

          <td width="80%">
            <table>
              <label for="txtPhoneLines">Phone lines</label>
            </table>
          </td>
        </tr>
      </table>

      <br />

      <br />
      <label for="cmbCellPhones">Cell phones:</label>
      <br />
      <table>
        <tr>
          <td>
            <select id="txtCellPhones" size="5" v-model="selectedPhone">
              <option
                v-for="phone in cellPhones"
                :key="phone.value"
                :value="phone.value"
              >{{phone.name}}</option>
            </select>
          </td>
          <td>
            <input type="button" value="&gt;" @click="addPhone" />
            <br />
            <input type="button" value="&lt;" @click="removePhone" />
          </td>
          <td>
            <select id="txtChosenCellPhones" size="5" style="width: 150px" v-model="selectedPhoneBasket">
              <option
                v-for="phone in chosenCellPhones"
                :key="phone.value"
                :value="phone.value"
              >{{phone.name}}</option>
            </select>
          </td>
        </tr>
      </table>
      <br />
      <div align="center">
        <p>Total price: {{totalPrice}} dkk</p>
        <button class="button button1" @click="buy">BUY NOW</button>
      </div>
    </div>
  </section>
</template>

<script>
import methods from "../methods/methods";
export default {
  data() {
    return {
      totalPrice: 0,
      internet: false,
      oldNumberOfLines: 0,
      numberOfLines: 0,
      selectedPhone: "",
      selectedPhoneBasket: "",
      cellPhones: [
        {
          name: "Motorola G99",
          value: "moto",
          price: 800
        },
        {
          name: "iPhone 99",
          value: "iphone",
          price: 6000
        },
        {
          name: "Samsung Galaxy 99",
          value: "samsung",
          price: 1000
        },
        {
          name: "Sony Xperia 99",
          value: "sony",
          price: 900
        },
        {
          value: "huawei",
          name: "Huawei 99",
          price: 900
        }
      ],
      chosenCellPhones: []
    };
  },
  methods: {
    checkInternet() {
      if (this.internet) {
        this.totalPrice = methods.includeInternet(
          this.internet,
          200,
          this.totalPrice
        );
      } else {
        this.totalPrice = methods.excludeInternet(
          this.internet,
          200,
          this.totalPrice
        );
      }
    },
    phoneLines(){
      console.log(this.numberOfLines)
      if(this.numberOfLines > this.oldNumberOfLines){ 
        this.totalPrice = methods.incrementPhoneLine(this.numberOfLines, 150, this.totalPrice)
        this.oldNumberOfLines = this.numberOfLines
      }
      if(this.numberOfLines < this.oldNumberOfLines){
        this.totalPrice = methods.decrementPhoneLine(this.numberOfLines, 150, this.totalPrice)
        this.oldNumberOfLines = this.numberOfLines
      }
    },
    addPhone() {
      const phone = this.cellPhones.find(
        phone => phone.value === this.selectedPhone
      );
      if (phone) {
        this.totalPrice = methods.selectPhone(this.chosenCellPhones, phone, phone.price, this.totalPrice)
      }
      else{
        window.alert("Please select a phone!");
      }
    },
    removePhone() {
      const phone = this.chosenCellPhones.find(
        phone => phone.value === this.selectedPhoneBasket
      );
      if (phone) {
        this.totalPrice = methods.unselectPhone(this.chosenCellPhones, phone, phone.price, this.totalPrice)
      }
      else{
        window.alert("Please select a phone to remove!");
      }
    },
    buy(){
      if( this.internet != true && this.numberOfLines === 0 && this.chosenCellPhones.length === 0){
        window.alert("You haven't selected anything!");
      }
    }
  }
};
</script>

<style>
body {
  /* Photo by Donald Giannatti on Unsplash*/
  background-image: url("../assets/satelite.jpg");
  background-repeat: no-repeat;
  background-attachment: fixed;
  background-size: cover;
}

h1 {
  text-align: center;
  font-size: 1em;
  color: rgb(61, 30, 30);
  margin-bottom: 0.3em;
}

label {
  cursor: pointer;
  color: rgb(61, 30, 30);

  padding: 10px;
  margin: 3px;
}

table {
  margin: 0.3em;
  width: 290px;
}

th {
  padding: 0.2em;
}

td {
  padding-left: 0.5em;
  padding-right: 0.5em;
  border: 2px solid rgb(25, 98, 107, 0.1);
  background-color: rgb(191, 196, 176, 0.1);
}

#trHeader {
  color: rgb(61, 30, 30);
}

.centerCell {
  text-align: center;
}
section {
  background: rgb(191, 196, 176, 0.5);
  color: white;
  border-radius: 1em;
  padding: 1em;
  position: absolute;
  top: 50%;
  left: 50%;
  margin-right: -50%;
  transform: translate(-50%, -50%);
}

.button {
  background-color: #4caf50; /* Green */
  border: none;
  color: white;
  padding: 10px 10px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  transition-duration: 0.4s;
  cursor: pointer;
}

.button1 {
  background-color: white;
  color: black;
  border: 2px solid #4caf50;
}

.button1:hover {
  background-color: #4caf50;
  color: white;
}

.button2 {
  background-color: white;
  color: black;
  border: 2px solid #008cba;
}

.button2:hover {
  background-color: #008cba;
  color: white;
}

.button3 {
  background-color: white;
  color: black;
  border: 2px solid #f44336;
}

.button3:hover {
  background-color: #f44336;
  color: white;
}

.button4 {
  background-color: white;
  color: black;
  border: 2px solid #e7e7e7;
}

.button4:hover {
  background-color: #e7e7e7;
}

.button5 {
  background-color: white;
  color: black;
  border: 2px solid #555555;
}

.button5:hover {
  background-color: #555555;
  color: white;
}

#MainTable {
  width: 100%;
  background-color: #d8f0da;
  border: 1px;
  min-width: 100%;
  position: relative;
  opacity: 0.97;
  background: transparent;
}
</style>