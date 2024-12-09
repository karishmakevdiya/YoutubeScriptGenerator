<template>
  <!-- <div class="item"> -->
    <div class="">
    <div class="split left">
      <form @submit.prevent="GenerateIntro">
        <label>Add video script</label>
        <input type="text" placeholder="Add video script here..." v-model="script">
        <label v-if="isShow">Please Provide some texts</label>
        <input type="submit" >
      </form>
    </div>

    <div class="split right">
      <!-- <div id="myIntro">{{ FinalIntro }}</div> -->
      
      <label>OUTPUT</label>

        <textarea id="myIntro" ref="myIntro" placeholder="" v-model="this.FinalIntro"></textarea>
        <button class="copyText" :onclick="CopyIntro" >{{ this.copyText }}</button>
    
      </div>
    
  </div>
</template>
<script>
// import HelloWorld from './components/HelloWorld.vue'
// import TheWelcome from './components/TheWelcome.vue'

export default {

  props: [
    'apikey'
  ],

  data: () => {

    return {
      script: '',
      FinalIntro:'',
      apiUrl: 'https://api.groq.com/openai/v1/chat/completions',
      token: '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',
      copyText: 'Copy text',
      isShow: false
    }

  },
  computed:{

  },

  methods:{
    CopyIntro(){
      var Url = this.$refs.myIntro;
      console.log(Url.value)
      Url.select();
      document.execCommand("copy");
      this.copyText = 'coppied'
      setTimeout(() => this.copyText = 'Copy text', 2000);

      

    },
    GenerateIntro(){
      if(this.script == ''){
this.isShow = true;
return;
      }
      this.isShow = false;
      var ItemJSON = 
        {
          "messages": [{
          "role": "user", 
          "content": "Please provide a short and concise summary of the following text: " + this.script
        }], 
          "model": "llama3-8b-8192"
        }
      // console.log(ItemJSON);
var $this = this
      var xmlhttp = new XMLHttpRequest();
      // xmlhttp.responseType = 'json';
    xmlhttp.open("POST",this.apiUrl, false);
   
    xmlhttp.setRequestHeader("Content-Type", "application/json");
    xmlhttp.setRequestHeader('Authorization', 'Bearer ' + this.token); //in prod, you should encrypt user name and password and provide encrypted keys here instead 
    
    xmlhttp.onload  = function() {
   var jsonResponse = JSON.parse(xmlhttp.responseText);
   $this.FinalIntro = jsonResponse.choices[0].message.content;
   console.log(this.FinalIntro)
   console.log(JSON.parse(xmlhttp.responseText))
   // do something with jsonResponse
};
xmlhttp.send(JSON.stringify(ItemJSON));
    // console.log(typeof(xmlhttp.responseText));


// console.log(this.script);

    }
  },

  created(){
  },
  mounted(){
  }

}
</script>

<style scoped lang="scss">
.split {
  height: 100%;
  width: 50%;
  position: fixed;
  z-index: 1;
  top: 0;
  overflow-x: hidden;
  padding-top: 20px;
  padding: 50px;
}

label{
  color:#fff;
}

.left {
  left: 0;
}

.right {
  right: 0;
}

textarea {
  width: 100% !important;
  height: 150px;
  padding: 12px 20px;
  box-sizing: border-box;
  border: 2px solid #ccc;
  border-radius: 4px;
  background-color: #f8f8f8;
  font-size: 16px;
  resize: none;
}
input[type=text], select {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

input[type=submit],.copyText {
  width: 100%;
  background-color: #325233;
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

input[type=submit]:hover, .copyText:hover {
  background-color: #577859;
  border: 1px solid #898888;
}
.item {
  margin-top: 2rem;
  display: flex;
  position: relative;
}

.details {
  flex: 1;
  margin-left: 1rem;
}

i {
  display: flex;
  place-items: center;
  place-content: center;
  width: 32px;
  height: 32px;

  color: var(--color-text);
}

h3 {
  font-size: 1.2rem;
  font-weight: 500;
  margin-bottom: 0.4rem;
  color: var(--color-heading);
}

@media (min-width: 1024px) {
  .item {
    margin-top: 0;
    padding: 0.4rem 0 1rem calc(var(--section-gap) / 2);
  }

  i {
    top: calc(50% - 25px);
    left: -26px;
    position: absolute;
    border: 1px solid var(--color-border);
    background: var(--color-background);
    border-radius: 8px;
    width: 50px;
    height: 50px;
  }

  .item:before {
    content: ' ';
    border-left: 1px solid var(--color-border);
    position: absolute;
    left: 0;
    bottom: calc(50% + 25px);
    height: calc(50% - 25px);
  }

  .item:after {
    content: ' ';
    border-left: 1px solid var(--color-border);
    position: absolute;
    left: 0;
    top: calc(50% + 25px);
    height: calc(50% - 25px);
  }

  .item:first-of-type:before {
    display: none;
  }

  .item:last-of-type:after {
    display: none;
  }
}
@media (max-width: 750px) {
  .split{
    padding: 20px !important;
  }
  
}
</style>
