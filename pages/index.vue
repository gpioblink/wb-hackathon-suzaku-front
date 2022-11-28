<template>
  <v-row justify="center" align="center">
    <!--<v-col  cols="12">
      <v-alert type="info">
        現在別のジョブが実行中のためジョブを作成できません。終了までお待ちください。
      </v-alert>
    </v-col>-->
    <v-col cols="12" lg="8">
      <v-card>
        <v-card-title class="headline">
         フレンドリスト
        </v-card-title>

        <v-card-text>
          <p>話したい友達の名前をクリックするとビデオチャットが起動します。</p>
          <p>相手からの呼び出しがある場合、右のランプが点灯します。</p>

        <v-list
          subheader
          two-line
        >

          <v-list-item link 
            v-for="friend in friends"
            :key="friend.title"
            @click.stop="friend.isDialogOpen = true"
          >
            <v-list-item-avatar>
              <v-icon
                class="grey lighten-1"
                dark
                v-text="friend.icon"
              >
              </v-icon>
            </v-list-item-avatar>

            <v-list-item-content>
              <v-list-item-title v-text="friend.name"></v-list-item-title>
            </v-list-item-content>

            <v-list-item-action>
                <v-icon v-if="friend.isCalling" color="orange lighten-1">mdi-bell-ring</v-icon>
                <v-icon v-else-if="isInitial" color="gray lighten-1">mdi-vanish</v-icon>
                <v-icon v-else color="gray lighten-1">mdi-bell-outline</v-icon>
            </v-list-item-action>
          </v-list-item>

        </v-list>

        </v-card-text>
      </v-card>
    </v-col>

    <!-- TODO: ダイアログもちゃんとfriend配列読んでまともにやるなりする -->

    <v-dialog
      v-model="friends[0].isDialogOpen"
      max-width="600"
    >
      <v-card>
        <v-card-title class="text-h5">
          {{friends[0].name}}とのビデオチャットを開始しますか?
        </v-card-title>
        <v-card-text>始めると相手側の人形に、光と振動で開始を通知します。</v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="gray darken-1"
            text
            @click="friends[0].isDialogOpen = false"
          >
            キャンセル
          </v-btn>
          <v-btn
            color="blue darken-1"
            outlined
            @click="startMeeting(friends[0]);friends[0].isDialogOpen = false"
          >
            始める
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>


    <v-dialog
      v-model="friends[1].isDialogOpen"
      max-width="600"
    >
      <v-card>
        <v-card-title class="text-h5">
          {{friends[1].name}}とのビデオチャットを開始しますか?
        </v-card-title>
        <v-card-text>始めると相手側の人形に、光と振動で開始を通知します。</v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="gray darken-1"
            text
            @click="friends[1].isDialogOpen = false"
          >
            キャンセル
          </v-btn>
          <v-btn
            color="blue darken-1"
            outlined
            @click="startMeeting(friends[1]);friends[1].isDialogOpen = false"
          >
            始める
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

  </v-row>
</template>

<script lang="ts">
  import {Component, Vue} from "nuxt-property-decorator";
  import axios from 'axios';

  const environment = process.env.NODE_ENV || "local";

  type FriendData = {
    id: string,
    name: string,
    icon: string,
    isCalling: boolean,
    isDialogOpen: boolean,
    meetingURL: string
  }

  // バックエンドURL
  // host: 192.168.0.18
  // エンドポイント: /:id/led, /:id/call
  // これらに必要なときにPOSTして、常にGETでpolling
  // {"flag": false}
  // LEDにPOSTを送る

  @Component // <- 1
  export default class Beta extends Vue /* <- 2 */{
    private host:string = (environment !== "development") ? "http://192.168.0.18" : "https://34c6-114-177-48-9.jp.ngrok.io";
    private isInitial:boolean = true;
    private lastOpenedMeetingId:string = "";
    private friends:FriendData[] = [
      {
        id: "1",
        icon: "mdi-flower",
        name: "山田さん",
        isCalling: false,
        isDialogOpen: false,
        meetingURL: "https://teams.microsoft.com/dl/launcher/launcher.html?url=%2F_%23%2Fl%2Fmeetup-join%2F19%3Ameeting_YWQwOGFkZjItODAxZC00ODJjLWI5YzktNDFiZmI1ZWEwYWNl%40thread.v2%2F0%3Fcontext%3D%257b%2522Tid%2522%253a%25224617a0ae-1a92-4482-a833-7bad535b3292%2522%252c%2522Oid%2522%253a%25226c873a17-3b3a-48d7-8fc2-3a82917a84c3%2522%257d%26anon%3Dtrue&type=meetup-join&deeplinkId=8fd865ef-2a91-4739-91a5-a6e2533ddef4&directDl=true&msLaunch=true&enableMobilePage=true&suppressPrompt=true"
      },
      {
        id: "2",
        icon: "mdi-cat",
        name: "鈴木さん",
        isCalling: false,
        isDialogOpen: false,
        meetingURL: "https://teams.microsoft.com/dl/launcher/launcher.html?url=%2F_%23%2Fl%2Fmeetup-join%2F19%3Ameeting_MmY1ZWE1NWMtN2ZiZi00ZmI0LWFiODItMmIwNzdkNTJjNDQ5%40thread.v2%2F0%3Fcontext%3D%257b%2522Tid%2522%253a%25224617a0ae-1a92-4482-a833-7bad535b3292%2522%252c%2522Oid%2522%253a%25226c873a17-3b3a-48d7-8fc2-3a82917a84c3%2522%257d%26anon%3Dtrue&type=meetup-join&deeplinkId=bbb84715-b586-4ca7-a4d6-49b01da99fcb&directDl=true&msLaunch=true&enableMobilePage=true&suppressPrompt=true"
      }
    ];

    async startMeeting(friend: FriendData) {
      // ベルをオフにする
      await axios.post(`${this.host}/${friend.id}/call`, { flag: false });

      // LEDを光らせる
      await axios.post(`${this.host}/${friend.id}/led`, { flag: true });

      // 会議終了検知を登録する
      this.lastOpenedMeetingId = friend.id;

      // リンクを新しいタブで開く
      window.open(friend.meetingURL, '_blank');
    }

    async updateNotification() {
      if(this.lastOpenedMeetingId === "") { // 会議を開いている間はバックエンド通信を止める
        for(let i = 0; i < this.friends.length; ++i) {
          // 全てのidに対してベルの状況を見て、鳴ってたらフロントに反映
          const res = await axios.get(`${this.host}/${this.friends[i].id}/call`);
          this.friends[i].isCalling = res.data.flag;
          console.log(res.data);
        }
      }
      this.isInitial = false;
    }

    async checkMeetingFinished() {
      if (this.lastOpenedMeetingId !== "" && !document.hidden) {
        // LEDをオフにする
        await axios.post(`${this.host}/${this.lastOpenedMeetingId}/led`, { flag: false });
        console.log(`meeting id ${this.lastOpenedMeetingId} has closed.`);
        this.lastOpenedMeetingId = "";
      }
    }

    async mounted() {
      setInterval( () => {
        this.updateNotification();
        this.checkMeetingFinished();
      }
      , 1000);
    }

  }
</script>