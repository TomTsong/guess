<template>
  <div>
    <div><span>Please input:</span></div>
    <div><textarea v-model.lazy="text" id="text">{{ text }}</textarea></div>
    <div id="result">
      <div><textarea id="arr">{{ result.arr }}</textarea></div>
      <div><textarea id="js">{{ result.js }}</textarea></div>
      <div><textarea id="headers">{{ result.headers }}</textarea></div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Fun',
    data(){
      return {
        text: '',
        result: {
          arr: "",
          js: "",
          headers: ""
        }
      }
    },
    methods: {
      parseToArray(){
        let arr = null;
        try{
          arr = this.text.split(/\s+/)
        }catch(e){
          console(e)
        }
        return arr
      },
      parseToJson(){
        let True = true;
        let False = false;
        let nil = null;
        let Nil = null; 
        let Null = null; 
        let NULL = null;
        let None = null;
        let js = null;
        let start = this.text.indexOf('{');
        let end = this.text.lastIndexOf('}');
        let text = this.text;
        if (start === -1 || end === -1 || start > end){
          return js
        }
        text = text.substring(start, end + 1);
        try{
          eval("js=" + text);
        }catch(e){
          console.log(e)
        }
        return js
      },
      parseToHeaders(){
        let headers = {};
        let text = this.text;
        let lines = text.split('\n');
        if (lines.length === 0){
          return null
        }
        for (let line of lines){
          let index = line.indexOf(":");
          if (index === -1){
            return null
          }
          let key = line.substring(0, index).replace(/^[\'\" ]+/, "").replace(/[\'\" ]+$/, "");
          let value = line.substring(index + 1).replace(/^[\'\" ]+/, "").replace(/[\'\" ]+$/, "");
          headers[key] = value;
        }
        return headers
      },
    },
    //computed: {
      //result_arr(){
        //let arr = this.parseToArray();
        //arr = arr ? JSON.stringify(arr) : "";
        //return arr
      //},
      //result_js(){
        //let js = this.parseToJson();
        //js = js ? JSON.stringify(js) : "";
        //return js
      //},
      //result_headers(){
        //let headers = this.parseToHeaders();
        //headers = headers ? JSON.stringify(headers) : "";
        //return headers
      //}
    //},
    watch: {
      text: function(){
        let arr = this.parseToArray();
        arr = arr ? JSON.stringify(arr) : "";
        let js = this.parseToJson();
        js = js ? JSON.stringify(js) : "";
        let headers = this.parseToHeaders();
        headers = headers ? JSON.stringify(headers) : "";
        this.result.arr = arr;
        this.result.js = js;
        this.result.headers = headers
      }
    }
  }  
</script>
