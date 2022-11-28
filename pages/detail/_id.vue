<template>
  <div>
      <v-row justify="center" align="center">
        <v-col  cols="12" sm="10" md="8">
          <v-card>
            <v-card-title class="headline justify-center">
            {{jobData.id}} の IoTデバイス
            </v-card-title>
            <!-- <v-card-subtitle class="text-center">
            ジョブID: {{jobData.id}}
            </v-card-subtitle> -->
            <v-card-text>
              <v-card outlined color="grey">
                <!-- TODO: 表のように枠を付けたい -->
                <v-list>
                    <v-list-item>
                      <v-list-item-content>
                        <v-row>
                          <v-col cols="3" md="2" class="text-center align-self-center">
                            LEDの状態
                          </v-col>
                          <v-col cols="9" md="10">
                            {{jobData.ledState}}
                          </v-col>
                        </v-row>
                      </v-list-item-content>
                    </v-list-item>
                    <v-list-item>
                      <v-list-item-content>
                        <v-row>
                          <v-col cols="3" md="2" class="text-center align-self-center">
                            バイブの状態
                          </v-col>
                          <v-col cols="9" md="10">
                            {{jobData.vivState}}
                          </v-col>
                        </v-row>
                      </v-list-item-content>
                    </v-list-item>
                </v-list>
              </v-card>
            </v-card-text>
          </v-card>
        </v-col>

        <v-col v-if="jobData.state === '実行中'"  cols="12" sm="10" md="8">
          <v-alert type="info">
            このジョブは実行中です。ダウンロードしても全ての結果は反映されていませんのでご注意ください。
          </v-alert>
        </v-col>
        <!-- <v-col  cols="12" sm="10" md="8">
          <v-alert type="error">
            指定されたIDのジョブが見つかりませんでした。
          </v-alert>
        </v-col> -->

        <v-col  cols="12" sm="10" md="8">
          <v-card>
            <v-card-subtitle>
            IoTデバイスの制御
            </v-card-subtitle>
            <v-card-text>
              <v-row class="pa-2" align="center" justify="space-around">
                <v-btn disabled style="text-transform: none;" class="ma-2">LEDを光らせる</v-btn>
                <v-btn disabled class="ma-2">振動させる</v-btn>
              </v-row>
            </v-card-text>
          </v-card>
        </v-col>        
        <!-- <v-col  cols="12" sm="10" md="8">
          <v-card>
            <v-card-subtitle>
            カスタムクエリの実行
            </v-card-subtitle>
            <v-card-text>
              <p class="text-center">カスタムクエリを実行すると、実行結果をSQL-Likeな構文でより詳細に分析できます。</p>
              <p class="text-center">パラメータの入力欄に上記開始時刻と終了時刻の値を使用してください。</p>
              <v-row class="pa-2" align="center" justify="space-around">
                <v-btn :href="jobData.queryUrl" block style="text-transform: none;" class="ma-2">BigQuery で開く</v-btn>
              </v-row>
            </v-card-text>
          </v-card>
        </v-col> -->
      </v-row>
  </div>
</template>

<script lang="ts">
import axios from 'axios';
import { Component, Vue } from 'nuxt-property-decorator';
import camelcaseKeys from "camelcase-keys";

type JobData = {
  id: string,
  ledState: string,
  vivState: string,
}

@Component
export default class JobDetailPage extends Vue {
  jobData: JobData = {
    id: '',
    ledState: '',
    vivState: ''
  };

  async mounted() {
    await this.getJobDetail(this.$route.params.id);
  }

  async getJobDetail(jobId: string) {
    // const response = await axios.get("https://stock-search-go-dev-n4o7gzc7pa-an.a.run.app/job/"+jobId);
    // this.jobData = camelcaseKeys(response.data, {deep: true}) as JobData;
  }

  
}
</script>
