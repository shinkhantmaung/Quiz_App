<template>
  <div>
    <a-page-header style="border: 1px solid rgb(235, 237, 240)" title="Quiz App">
      <template slot="extra">
        <a-button key="1" type="primary" href="/">
          Restart
        </a-button>
      </template>
      <a-row type="flex" justify="space-around" align="middle">
        <h3>Counter: {{ numCorrect }}/{{ numTotal }}</h3>
      </a-row>
    </a-page-header>
  </div>
</template>
<style>
tr:last-child td {
  padding-bottom: 0;
}
</style>



<script>
export default {
  props: [
    'numCorrect',
    'numTotal'
  ]
}
</script>
