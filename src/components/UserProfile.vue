<template>
    <div class="user-profile">
        <div class="user-profile__user-panel">
            <h1 class="user-profile__username"> @{{user.username}}</h1>
            <div class="user-profile__admin-badge" v-if="user.isAdmin">
                Admin
            </div>
            <div class="user_profile__follow-count">
                <strong>Followers: </strong> {{ followers }}
            </div>
            <form class="user-profile__create-twoot" @submit.prevent="createNewTwoot" :class="{'--exceeded': newTwootCharacterCount > 180}">
                <label for="newTwoot"><strong>New Twoot</strong> ({{newTwootCharacterCount}} / 180)</label>
                <textarea name="" id="newTwoot" cols="30" rows="4" v-model="newTwootContent"></textarea>

                <div class="user-profile__create-twoot-type">
                    <label for="newTwootType"><strong>Type: </strong></label>
                    <select id="newTwootType" v-model="selectedTwootType">
                        <option class="" :value="option.value" v-for="(option, index) in twootTypes" :key="index">
                            {{ option.name }}
                        </option>
                    </select>
                </div>
                <button>Twoot!</button>
            </form>
        </div>
        <div class="user-profile__twoots-wrapper">
            <TwootItem
                    v-for="twoot in user.twoots"
                    :key="twoot.id"
                    :username="user.username"
                    :twoot="twoot"
                    @favorite="toggleFavorite"
            />
        </div>

    </div>
</template>

<script>
    import TwootItem from "./TwootItem";

    export default {
        name: "UserProfile",
        components: {TwootItem},

        data() {
            return {
                newTwootContent: '',
                selectedTwootType: 'Draft',
                twootTypes: [
                    {value: 'draft', name: 'Draft'},
                    {value: 'instant', name: 'Instant Twoot'}
                ],
                followers: 0,
                user: {
                    id: 1,
                    username: 'pamila',
                    firstName: 'Miro',
                    lastName: 'Pantos',
                    email: 'pamila@sirovo.com',
                    isAdmin: true,
                    twoots: [
                        {id: 1, content: 'Twotter is garbage!'},
                        {id: 2, content: 'Twotter is meh'}
                    ]
                }
            }
        },
        watch: {
            followers(newFollowerCount, oldFollowerCount) {
                if (oldFollowerCount < newFollowerCount) {
                    console.log(`${this.user.username} has gained a follower`)
                }
            }
        },
        computed: {
            newTwootCharacterCount(){
                return this.newTwootContent.length;
            }
            },
        methods:
            {
                followUser() {
                    this.followers++
                },

                createNewTwoot() {
                    if (this.newTwootContent && this.selectedTwootType !== 'Draft'){
                        this.user.twoots.unshift({
                            id: this.user.twoots.length +1,
                            content: this.newTwootContent
                        })
                        this.newTwootContent = '';
                    }
                        }

           },

        mounted() {
            this.followUser();
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
