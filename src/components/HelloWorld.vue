<template>
  <div class="container">
		<img src="../assets/gpt.png" alt="logo" />
		<div class="text-contain">
			<h1>Ask me anything!!!</h1>
			<i class="fa-solid fa-ellipsis fa-beat three_dots" v-if="dots"></i>
			<div v-else class="output">
        {{output}}
			</div>
		</div>
		<div class="text-btn">
			<textarea v-model="input" ></textarea>
			<i class="fa-brands fa-telegram fa-fade send-btn" @click="btn"></i>
		</div>
	</div>

</template>

<script>
 import { Configuration, OpenAIApi } from "openai";
 import { ref } from "vue";


export default {
  name: 'HelloWorld',
  setup(){
    const input = ref("")
    const output = ref("")
    const dots = ref(false)



    const configuration = new Configuration({
      apiKey: process.envSample.VUE_APP_API_KEY,
    });

    const openai = new OpenAIApi(configuration);
    async function callApi(input){
      const response = await openai.createCompletion({
        "model": "text-davinci-003",
        "prompt": `${input}`,
        max_tokens: 700
      });

      if(response){
        dots.value = false 
      }else{
        console.error("An error has occurred!");
        output.value = "An error has occurred!";
      }
      output.value = response.data.choices[0].text
      console.log(response)
    }

    function btn(){
      callApi(input.value)
      dots.value = true 
      input.value = ""
    }

    return{
      input,
      output,
      callApi,
      btn,
      dots
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>


.container{
	display: flex;
	align-items: center;
	justify-content: center;
	flex-direction: column;
	margin-top: 15vh;
}

img{
	max-width: 7vw;
}

.text-contain{
	display: flex;
	align-items: center;
	justify-content: center;
	flex-direction: column;
}

h1{
	color: aliceblue;
}

.three_dots{
	font-size: 40px;
}

.output{
	width: 70vw;
	word-wrap: break-word;
	text-align: left;
	color: aliceblue;
	font-size: 17px;
}

.text-btn{
	display: flex;
	align-items: center;
	justify-content: center;
	margin-top: 5vh;
}



textarea{
	width: 50vw;
	margin-right: 0.5vw;
}


.send-btn{
	font-size: 40px;
	color: rgb(252, 222, 222);
	cursor: pointer;
}



</style>
