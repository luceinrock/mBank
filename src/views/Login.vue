<template>
    <ion-page>
        <ion-header>
            <ion-toolbar>
                <ion-title>My Account</ion-title>
            </ion-toolbar>
        </ion-header>
        <ion-content :fullscreen="true">
            <ion-header collapse="condense">
                <ion-toolbar>
                    <ion-title size="large">My Account</ion-title>
                </ion-toolbar>
            </ion-header>
            <ion-card>
                <ion-card-header>
                    <ion-card-subtitle>Account Name</ion-card-subtitle>
                    <ion-card-title>{{ userName }} </ion-card-title>
                </ion-card-header>
                <ion-card-content>
                    <ion-item>
                        <ion-label>
                            Available Balance: {{ balance }}
                        </ion-label>
                    </ion-item>
                    <ion-item>
                        <ion-label position="stacked">
                            Amount To Deposit:
                        </ion-label>
                        <ion-input
                            v-model="myBalance"
                            inputmode="decimal"
                            type="number"
                        >
                        </ion-input>
                    </ion-item>
                    <section>
                        {{ msg }}
                    </section>
                    <section>
                        <ion-button color="success" @click="deposit"
                            >Deposit</ion-button
                        >
                    </section>
                </ion-card-content>
            </ion-card>
        </ion-content>
    </ion-page>
</template>

<script>
import {
    IonPage,
    IonHeader,
    IonToolbar,
    IonTitle,
    IonCard,
    IonContent,
    IonCardHeader,
    IonCardSubtitle,
    IonCardTitle,
    IonCardContent,
    IonLabel,
    IonInput,
    IonItem,
    IonButton,
} from "@ionic/vue";
import axios from "axios";

export default {
    name: "Tab1",
    components: {
        IonHeader,
        IonToolbar,
        IonTitle,
        IonContent,
        IonPage,
        IonCard,
        IonCardHeader,
        IonCardSubtitle,
        IonCardTitle,
        IonCardContent,
        IonLabel,
        IonInput,
        IonItem,
        IonButton,
    },
    data() {
        return {
            info: null,
            balance: 0,
            userName: null,
            msg: null,
            myBalance: 0.0,
            currentUser: 1,
        };
    },
    mounted() {
        axios
            .get(
                "http://rent.cebu-airtraffic.com/fw/api/myBalance/" +
                    this.currentUser
            )
            .then(
                (response) => (
                    (this.info = response.data.balance),
                    (this.balance = this.info[0].balance),
                    (this.userName = this.info[0].user_name)
                )
            );
    },
    methods: {
        deposit() {
            axios
                .get(
                    "http://rent.cebu-airtraffic.com/fw/api/deposit/" +
                        this.myBalance +
                        "/" +
                        this.currentUser,
                )
                .then(
                    (response) => (
                        (this.info = response.data.balance),
                        (this.balance = this.info.balance[0].balance),
                        (this.userName = this.info.balance[0].user_name)
                    )
                );
            this.myBalance = 0;
        },
    },
};
</script>