<template>
  <el-container>
    <el-aside width="200px"></el-aside>
    <el-container>
      <el-header></el-header>
      <el-main>
        <div><span>Please input:</span></div>
        <el-input
          type="textarea"
          :rows="5"
          v-model.lazy="text">
        </el-input>
        <div>
          <div><span>To json:</span></div>
          <div id="js"><pre>{{ result.js }}</pre></div>
          <div><span>To array:</span></div>
          <div id="arr"><pre>{{ result.arr }}</pre></div>
          <div><span>To headers:</span></div>
          <div id="headers"><pre>{{ result.headers }}</pre></div>
        </div>
      </el-main>
    </el-container>
    <el-aside width="200px"></el-aside>
  </el-container>
</template>

<style>
  pre {
    border: solid 1px gray;
    white-space: pre-wrap;
    word-wrap: break-word;
  }
  
</style>

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
          arr = this.text.trim().split(/\s+/)
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
        let text = this.text.trim();
        let start = text.indexOf('{');
        let end = text.lastIndexOf('}');
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
        let text = this.text.trim();
        let lines = text.split('\n');
        if (lines.length === 0){
          return null
        }

        for (let line of lines){
          line = line.trim();
          if (!line){
            continue
          }

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
        arr = arr ? JSON.stringify(arr, null, 2) : "";
        let js = this.parseToJson();
        js = js ? JSON.stringify(js, null, 2) : "";
        let headers = this.parseToHeaders();
        headers = headers ? JSON.stringify(headers, null, 2) : "";
        this.result.arr = arr;
        this.result.js = js;
        this.result.headers = headers
      }
    }
  }  
</script>
