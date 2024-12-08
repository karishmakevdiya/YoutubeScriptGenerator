<template>
  <div class="item">
    <div>
      <form @submit.prevent="GenerateIntro">
        <input type="text" placeholder="search.." v-model="script">
      </form>
    </div>

    <div>
      <!-- <div id="myIntro">{{ FinalIntro }}</div> -->
      <button :onclick="CopyIntro">Copy text</button>

        <textarea id="myIntro" ref="myIntro" placeholder="" v-model="this.FinalIntro"></textarea>
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
      FinalIntro:'test',
      apiUrl: 'https://api.groq.com/openai/v1/chat/completions',
      token: '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'
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

    },
    GenerateIntro(){
      
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

<style scoped>
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
</style>
