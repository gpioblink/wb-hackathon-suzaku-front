<template>
  <v-row justify="center" align="center">
    <!--<v-col  cols="12">
      <v-alert type="info">
        現在別のジョブが実行中のためジョブを作成できません。終了までお待ちください。
      </v-alert>
    </v-col>-->
    <v-col cols="8">
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
          山田さんとのビデオチャットを開始しますか?
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
            @click="friends[0].isDialogOpen = false"
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
          鈴木さんとのビデオチャットを開始しますか?
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
            @click="friends[1].isDialogOpen = false"
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

  type FriendData = {
    id: string,
    name: string,
    icon: string,
    isCalling: boolean,
    isDialogOpen: boolean
  }

  // バックエンドURL
  // host: 192.168.0.18
  // エンドポイント: /:id/led, /:id/call
  // これらに必要なときにPOSTして、常にGETでpolling
  // {"flag": false}
  // LEDにPOSTを送る

  @Component // <- 1
  export default class Beta extends Vue /* <- 2 */{
    private valid = true;
    private queryRaw = "";
    private friends:FriendData[] = [
      {
        id: "1",
        icon: "mdi-flower",
        name: "山田さん",
        isCalling: false,
        isDialogOpen: false
      },
      {
        id: "1",
        icon: "mdi-cat",
        name: "鈴木さん",
        isCalling: true,
        isDialogOpen: false
      }
    ];

    async sendQueries() {
      // await axios.post(`https://192.168.0.13/${this.queryRaw}`);
      this.moveToDetailPage(this.queryRaw);
    }

    moveToDetailPage(id: string) {
      this.$router.push({path: `/detail/${id}`})
    }


  }
</script>