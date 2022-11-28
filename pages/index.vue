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

          <v-list-item link @click.stop="dialog_a = true">
            <v-list-item-avatar>
              <v-icon
                class="grey lighten-1"
                dark
              >
                mdi-flower
              </v-icon>
            </v-list-item-avatar>

            <v-list-item-content>
              <v-list-item-title>山田さん</v-list-item-title>

              <!-- <v-list-item-subtitle></v-list-item-subtitle> -->
            </v-list-item-content>

            <v-list-item-action>
                <v-icon color="gray lighten-1">mdi-bell-outline</v-icon>
            </v-list-item-action>
          </v-list-item>

          <v-list-item link @click.stop="dialog_b = true">
            <v-list-item-avatar>
              <v-icon
                class="grey lighten-1"
                dark
              >
                mdi-cat
              </v-icon>
            </v-list-item-avatar>

            <v-list-item-content>
              <v-list-item-title>鈴木さん</v-list-item-title>

              <!-- <v-list-item-subtitle></v-list-item-subtitle> -->
            </v-list-item-content>

            <v-list-item-action>
                <v-icon color="orange lighten-1">mdi-bell-ring</v-icon>
            </v-list-item-action>
          </v-list-item>

        </v-list>

        </v-card-text>
      </v-card>
    </v-col>

    <v-dialog
      v-model="dialog_a"
      persistent
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
            @click="dialog_a = false"
          >
            キャンセル
          </v-btn>
          <v-btn
            color="blue darken-1"
            outlined
            @click="dialog_a = false"
          >
            始める
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>


    <v-dialog
      v-model="dialog_b"
      persistent
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
            @click="dialog_b = false"
          >
            キャンセル
          </v-btn>
          <v-btn
            color="blue darken-1"
            outlined
            @click="dialog_b = false"
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
    isCalling: boolean,
    isDialogOpen: boolean
  }

  @Component // <- 1
  export default class Beta extends Vue /* <- 2 */{
    private valid = true;
    private queryRaw = "";
    private dialog_a:boolean = false;
    private dialog_b:boolean = false;

    async sendQueries() {
      // await axios.post(`https://192.168.0.13/${this.queryRaw}`);
      this.moveToDetailPage(this.queryRaw);
    }

    moveToDetailPage(id: string) {
      this.$router.push({path: `/detail/${id}`})
    }
  }
</script>