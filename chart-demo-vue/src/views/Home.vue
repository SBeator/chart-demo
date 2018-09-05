<template>
  <Layout>
    <Header></Header>
    <Content>
      <Demo />
    </Content>
    <Footer></Footer>
  </Layout>
</template>

<script>
// @ is an alias to /src
import Demo from '@/components/Demo.vue'

export default {
  name: 'home',
  components: {
    Demo
  }
}
</script>
