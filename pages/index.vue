<template>
  <section class="section">
    <div class="columns is-mobile">
      <b-field class="file">
        <b-upload v-model="file">
          <a class="button is-primary">
            <b-icon icon="upload"></b-icon>
            <span>Selecione o documento</span>
          </a>
        </b-upload>
        <span class="file-name" v-if="file">{{ file.name }}</span>
      </b-field>
    </div>
    <hr />
    <codemirror v-model="result" :options="cmOption"></codemirror>
  </section>
</template>

<script>

export default {
  name: "HomePage",

  components: {  },

  data() {
    return {
      file: null,
      result: '{}',
      cmOption: {
        mode: "text/javascript",
        tabSize: 4
      }
    };
  },
  watch: {
    file() {
      console.log(this.file);
      var formData = new FormData();
      formData.append("file", this.file);
      this.$axios
        .$post("https://api.dochub.com.br/document", formData, {
          headers: {
            "Content-Type": "multipart/form-data"
          }
        })
        .then(response => (this.result = JSON.stringify(response, null, 2)));
    }
  }
};
</script>
