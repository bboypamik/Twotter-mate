<template>
    <div class="user-profile">
        <div class="user-profile__user-panel">
            <h1 class="user-profile__username"> @{{state.user.username}}</h1>
            <h3>{{userId}}</h3>
            <div class="user-profile__admin-badge" v-if="state.user.isAdmin">
                Admin
            </div>
            <div class="user_profile__follow-count">
                <strong>Followers: </strong> {{ state.followers }}
            </div>
            <form class="user-profile__create-twoot" @submit.prevent="createNewTwoot"
                  :class="{'--exceeded': newTwootCharacterCount > 180}">
                <label for="newTwoot"><strong>New Twoot</strong> ({{newTwootCharacterCount}} / 180)</label>
                <textarea name="" id="newTwoot" cols="30" rows="4" v-model="state.newTwootContent"></textarea>

                <div class="user-profile__create-twoot-type">
                    <label for="newTwootType"><strong>Type: </strong></label>
                    <select id="newTwootType" v-model="state.selectedTwootType">
                        <option class="" :value="option.value" v-for="(option, index) in state.twootTypes" :key="index">
                            {{ option.name }}
                        </option>
                    </select>
                </div>
                <button>Twoot!</button>
            </form>
        </div>
        <div class="user-profile__twoots-wrapper">
            <TwootItem
                    v-for="twoot in state.user.twoots"
                    :key="twoot.id"
                    :username="state.user.username"
                    :twoot="twoot"

            />
        </div>

    </div>
</template>

<script>
    import {computed, reactive} from 'vue';
    import { useRoute } from 'vue-router';
    import TwootItem from "../components/TwootItem";
    import {users} from "../assets/users";

    export default {
        name: "UserProfile",
        components: {TwootItem},

        setup() {

            const route = useRoute();
            const userId = computed(() => route.params.userId);

            const state = reactive({

                user: users[userId.value - 1] || users[0],
                newTwootContent: '',
                selectedTwootType: 'Draft',
                twootTypes: [
                    {value: 'draft', name: 'Draft'},
                    {value: 'instant', name: 'Instant Twoot'}
                ],
                followers: 0,
            });

            const newTwootCharacterCount = computed(() => state.newTwootContent.length);



            function createNewTwoot() {
                if (state.newTwootContent && state.selectedTwootType !== 'Draft') {
                    state.user.twoots.unshift({
                        id: state.user.twoots.length + 1,
                        content: state.newTwootContent
                    })
                    this.newTwootContent = '';
                }
            }




            return {
                state,
                newTwootCharacterCount,
                createNewTwoot,
                userId
            }
        }
    }

</script>

<style lang="scss" scoped>
    .user-profile {
        display: grid;
        grid-template-columns: 1fr 3fr;
        width: 100%;
        padding: 50px 5%;

        .user-profile__user-panel {
            display: flex;
            flex-direction: column;
            justify-content: right;
            margin-right: 50px;
            padding: 20px;
            background-color: white;
            border-radius: 5px;
            border: 1px solid #DFE3E8;


            h1 {
                margin: 0;
            }

            .user-profile__admin-badge {
                background: purple;
                color: white;
                border-radius: 5px;
                margin: auto;
                padding: 10px;
                margin: 10px 0px 10px 0px;

            }

            .user-profile__create-twoot {
                padding-top: 20px;
                display: flex;
                flex-direction: column;

                &.--exceeded {
                    color: red;
                    border-color: red;

                    button {
                        background-color: red;
                    }
                }
            }
        }

        .user-profile__twoots-wrapper {
            display: grid;
            grid-gap: 10px;
        }
    }


</style>
