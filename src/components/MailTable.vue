<template>
    <table class="mail-table">
        <tbody>
            <tr v-for="email in unarchivedEmails" :key="email.id" :class="['clickable', email.read ? 'read' : '']"
                @click="openEmail(email)">
                <td><input type="checkbox" /></td>
                <td>{{ email.from }}</td>
                <td>
                    <p><strong>{{ email.subject }}</strong> - {{ email.body }}</p>
                </td>
                <!-- <td class="date">{{format(new Date(email.sentAt), 'MMM do yyyy')}}</td> -->
                <td class="date">{{ format(new Date(email.sentAt),'MMM do yyyy')}}
                </td>
                <td><button @click="archieveEmail(email)">Archieve</button></td>
            </tr>
        </tbody>
    </table>
    <MailView v-if="openedEmail" :email="openedEmail" />

</template>

<script>
import axios from 'axios';
import { format } from 'date-fns'
import MailView from './MailView.vue'
export default {
    async setup() {
        let { data: emails } = await axios.get('http://localhost:3000/emails')
        return {
            format,
            emails,
            openedEmail: null
        }
    },
    components:{
        MailView,
    },
    computed: {
        sortedEmails() {
            return this.emails.sort((e1, e2) => {
                return e1.sentAt < e2.sentAt ? 1 : -1
            })
        },
        unarchivedEmails() {
            return this.sortedEmails.filter(e => e.archived)
        }
    },
    methods: {
        openEmail(email) {
            email.read = true
            this.updateEmail(email)
            this.openedEmail = email
        },
        archieveEmail(email){
            email.archived = true
            this.updateEmail(email)
        },
        updateEmail(email){
            axios.put(`http://localhost:3000/emails/${email.id}`,email)
        }
    }
}
</script>

<style lang="scss" scoped></style>