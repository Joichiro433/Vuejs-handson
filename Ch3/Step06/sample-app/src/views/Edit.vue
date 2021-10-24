<template>
<div>
    <div :class="$style.inputContainer">
        <form :class="$style.inputArea">
            <p>ユーザー登録</p>
            <label>ニックネーム</label>
            <input v-model="nickname" />
            <label>メールアドレス</label>
            <input v-model="email" />
        </form>

        <div :class="$style.previewArea">
            <p>プレビュー</p>
            <label>ニックネーム</label>
            <input v-bind:value="nickname" readonly />
            <label>メールアドレス</label>
            <input v-bind:value="email" readonly />
        </div>

        <div :class="$style.buttonArea">
            <button @click="saveUser">ユーザーを登録する</button>
        </div>
    </div>

    <div>
        <div :class="$style.filterArea">
            <label>リストをニックネームで絞り込む</label>
            <input v-model="nicknameFilter" />
        </div>
        <table>
            <thead>
                <th>ニックネーム</th>
                <th>メールアドレス</th>
            </thead>
            <tbody>
                <tr 
                    v-is="user-row" 
                    v-for="(user, index) in filteredUsers" 
                    :key="index" 
                    :user="user" 
                />
            </tbody>
        </table>
        <div :class="$style.buttonWrapper">
            <button @click="displayUsers">ユーザーを表示する</button>
        </div>
    </div>
</div>
</template>

<script>
import {defineComponent, reactive, toRefs, computed} from '@vue/composition-api';
import userRowComponent, {User} from '@/components/UserRow.vue';

export default defineComponent({
    components: {
        'user-row': userRowComponent,
    },

    setup() {
        const state = reactive({
            users: [],
            nickname: '',
            email: '',
            nicknameFilter: '',
            filteredUsers: computed(() => {
                return state.users.filter(user => user.nickname.includes(state.nicknameFilter));
            }),
        });

        const saveUser = () => {
            const user = new User(state.nickname, state.email);
            state.users.push(user);

            alert('nickname: ' + state.nickname + ', email: ' + state.email);
        };

        const displayUsers = () => {
            let message = state.users.length + '人のユーザーが登録されています。';
            for (const user of state.users) {
                message += '\n' + user.nickname;
            }
            alert(message);
        };

        return {
            ...toRefs(state),
            saveUser,
            displayUsers,
        };
    },
});
</script>

<style module lang="scss">
.inputContainer {
    display: grid;
    grid-template-rows: 200px 50px;
    grid-template-columns: 300px 300px;
    grid-template-areas: 
        'inputArea previewArea' 
        'buttonArea buttonArea';
    grid-gap: 10px;}
.inputArea{
    grid-area: inputArea;
    display: grid;
}
.previewArea {
    grid-area: previewArea;
    display: grid;
}
.buttonArea {
    grid-area: buttonArea;
    display: grid;
    justify-items: conter;
    align-items: center;
}
.filterArea {
    display: grid;
    grid-template-columns: 300px 300px;
    grid-gap: 10px;
}
table {
    margin-top: 10px;
    width: 610px;
    border-collapse: collapse;
}
th, td {
    width: 50%;
    padding-left: 5px;
    word-break: break-all;
}
.buttonWrapper {
    width: 610px;
    height: 50px;
    display: grid;
    justify-content: conter;
    align-items: center;
}
</style>