<template>
   <table class="mail-table">
    <tbody>
        <tr v-for="email in emails"
        :key="email.id"
        :class="['clickable',email.read ? 'read':'']"
        @click="email.read= true"
        >
        <td><input type="checkbox"/></td>
        <td>{{ email.from }}</td>
        <td><p><strong>{{ email.subject }}</strong> - {{ email.body }}</p></td>
        <!-- <td class="date">{{format(new Date(email.sentAt), 'MMM do yyyy')}}</td> -->
        <td class="date">{{ email.sentAt }}
        </td>
    </tr>
    </tbody>
</table>
</template>

<script>
import axios from 'axios';
export default {
    async  setup(){
        let { data: emails } = await axios.get('http://localhost:3000/emails')
        return {
         emails,
        }
  }
}
</script>

<style lang="scss" scoped>

</style>