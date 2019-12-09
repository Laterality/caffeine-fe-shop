<template>
<v-app>
    <v-content>
        <router-view/>
    </v-content>
    <v-snackbar
        class="mx-3 mb-2"
        v-model="showSnackbar">
        {{ snackbarText }}
        <v-btn
          color="pink"
          text
          @click="showSnackbar = false"
        >
          Close
        </v-btn>
    </v-snackbar>
</v-app>
</template>

<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'

@Component
export default class App extends Vue {
    private showSnackbar = false;
    private snackbarText = '';

    private messageSource: EventSource

    constructor () {
        super()
        this.messageSource = new EventSource('http://localhost:8080/notification/subscribe/shop?shopId=1', { withCredentials: true })
        this.messageSource.onmessage = this.handleNotification
    }

    private handleNotification (evt: MessageEvent) {
        this.snackbarText = evt.data
        this.showSnackbar = true
    }
}
</script>
