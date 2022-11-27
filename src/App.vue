<template>
  <div id="app">
    <div class="container">
      <div class="header">
        FAİZ HESAPLAMA BOTU
      </div>
      <div>
        <div class="content">
          <div class="leftText">
            Faiz Hesapla
          </div>
          <div class="rightField">
            Gelecekteki değer ve faiz tutarı
          </div>
        </div>
        <div class="content">
          <div class="leftText">
            Net Bugünkü Değer (Anapara)
          </div>
          <div class="rightfield">
            <label for="">
              <input v-model="anapara" class="anaparainput">
            </label>
          </div>
        </div>
        <div class="content">
          <div class="leftText">
            Faiz Oranı
          </div>
          <div class="rightfieldx">
              <input v-model="faiz" class="faizinput">
              <select class="selectOption" v-model="faizTipi">
                <option value="Yıllık" class="options">% Yıllık</option>
                <option value="Aylık" class="options">% Aylık</option>
                <option value="Günlük" class="options">% Günlük</option>
              </select>
          </div>
        </div>
        <div class="content">
            <div class="leftText">
              Faiz Dönemi
            </div>
            <div class="rightfieldx">
              <input v-model="donemdegeri" class="faizinput">
              <select v-model="donem" class="selectOption">
                <option class="options" v-for="item in donemArray" :value="item" :key="item">{{item}}</option>
              </select>
            </div>
        </div>
        <div class="hesaplabtn">
          <button :style="[buttonDisable ? disabledStyle : '']" :disabled="buttonDisable" @click="faizHesapla()" class="hesaplabuton">HESAPLA</button>
        </div>
        <div class="hesaplamasonucu" v-if="sonuc != 0">
          <span class="title">
            Hesaplama Sonucu
          </span>
          <label class="outText">
              ${{anapara}} kadar yatırım yaparsanız bu {{faiz}} faiz oranında ve {{donemdegeri}} bu kadar faiz döneminde <span class="sonuctext">{{sonuc}}</span> miktarı kadar para elde edeceksiniz
          </label>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return{
      anapara:null,
      faiz:0,
      faizTipi:"Yıllık",
      donemdegeri: 0,
      sonuc:0,
      donemArray:[
        'Yıl',
        'Ay',
        'Gün'
      ],
      disabledStyle:{
        background:'#ccc'
      },
      donem:"Yıl"
    }
  },
  methods:{
    faizHesapla(){
      const sonuc = this.anapara * Math.pow((1 + this.faiz*0.01),this.computedDonemDegeri);
      this.sonuc = parseInt(sonuc);
      console.log(sonuc);
    }
  },
  /*faiz dönemini verilen değere göre hesaplaalım */
  computed:{
    computedDonemDegeri(){
      if(this.faizTipi === "Yıllık"){
        if(this.donem === "Yıl")
        {
          return this.donemdegeri
        }else if(this.donem === "Ay"){
            return this.donemdegeri/12 //bu şekilde 12 aya bölünürse 1 yıllık faizin tamamını alırız
        }else{
          return this.donemdegeri/360
        }
      }else if(this.faizTipi === "Aylık"){
        if(this.donem === "Yıl")
        {
          return this.donemdegeri*12
        }else if(this.donem === "Ay"){
            return this.donemdegeri//bu şekilde 12 aya bölünürse 1 yıllık faizin tamamını alırız
        }else{
          return this.donemdegeri/30
        }
      }else{
        if(this.donem === "Yıl")
        {
          return this.donemdegeri*360
        }else if(this.donem === "Ay"){
            return this.donemdegeri*12//bu şekilde 12 aya bölünürse 1 yıllık faizin tamamını alırız
        }else{
          return this.donemdegeri
        }
      }
    },
    //formun içindeki inputlar boş ise butonu pasif yapalım
    buttonDisable(){
      return this.donemdegeri == 0 || this.faiz == 0 || this.sonuc === null
    }
  }

}
</script>

<style>
  .container{
    font-family: Arial, Helvetica, sans-serif;
    font-size: 18px;
    max-width: 600px;
    background-color: #f5f5f9;
    border-radius: 5px;
    box-shadow: 0 0 12px #888;
    margin-top: 20px;
    padding: 20px 10px 10px;
    margin-left: auto;
    margin-right: auto;
  }

  .header{
    font-weight: normal;
    padding-bottom: 5px;
    font-size: 17px;
    padding-top: 5px;
    width: 100%;
    text-shadow: 1px 1px 1px white;
    letter-spacing: 1px;
    text-align: center;
  }
  .content{
    margin-top: 20px;
    display: flex;
  }
  .leftText{
    width: 40%;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 15px;
    color: #004372;
  }
  .rightfield{
    font-size: 14px;
    width: 60%;
    display: flex;
    flex-direction: column;
  }
  .rightfieldx{
    font-size: 14px;
    width: 60%;
    display: flex;
  }
  .anaparainput{
    width: 95%;
    margin-top: 5px;
    margin-bottom: 5px;
    padding: 7px;
    border: solid 1px #e8e8e8;
    border-radius: 3px;
    box-shadow: 0 0 2px silver;
    font-size: 14px;
    resize: vertical;
  }
  .faizinput{
    margin-top: 5px;
    margin-bottom: 5px;
    padding: 7px;
    border: solid 1px #e8e8e8;
    border-radius: 3px;
    box-shadow: 0 0 2px silver;
    font-size: 14px;
    width: 60%;
  }
  .selectOption{
    padding: 7px;
    border: solid 1px #e8e8e8;
    border-radius: 3px;
    box-shadow: 0 0 1px silver;
    font-size: 12px;
    margin: 5px 4px 5px 10px;
    width: 115px;

  }
  .options{
    display: block;
    white-space: pre;
    min-height: 1.2em;
    padding: 0 2px 1px;
  }
  .hesaplabtn{
    border-top: 1px solid silver;
    display: flex;
    justify-content: flex-end;
    margin-top: 15px;
  }
  .hesaplabuton{
    box-sizing: content-box;
    padding: 10px;
    min-width: 100px;
    background-color: #0094ff;
    color: #fff;
    border-radius: 3px;
    cursor: pointer;
    border: solid 1px transparent;
    font-weight: 600;
    box-shadow: 1px 1px 1px #555555;
    position: relative;
    margin-top: 15px;
  }
  .hesaplamasonucu{
    max-width: 600px;
    background-color: #d8dbd3;
    box-shadow: 0 0 12px #888;
    border-radius: 7px;
    margin-top: 20px;
    padding: 20px 10px 10px;
    margin-right: auto;
    margin-left: auto;
    display: flex;
    flex-direction: column;
  }
  .title{
    text-align: center;
    font-family: Cambria, serif;
    border-bottom: 1px solid silver;
    padding-bottom: 10px;
    font-size: 20px;
    color: #23233b;
  }
  .outText{
    color: #313629;
    margin-top: 15px;
  }
  .sonuctext{
    font-weight: 600;
    font-size: 18px;
  }
</style>
