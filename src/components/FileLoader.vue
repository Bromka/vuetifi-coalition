<template>
  <v-container>
    <v-row>
      <div class="select_wrap">
        <p class="title_loader">
          Выбрать источник
        </p>
        <v-select v-model="setLocale"
                  :items="items"
                  dense
                  height="1.5em"
                  outlined>
          <template slot="item" slot-scope="data">
            <v-icon>{{ data.item.icon }}</v-icon>
            {{ data.item.value }}
          </template>
          <template slot="selection" slot-scope="data">
            <v-icon> {{ data.item.icon }}</v-icon>
            {{ data.item.value }}
          </template>
        </v-select>
      </div>
      <div class="loader_wrap">
        <p class="title_loader">
          {{ items.find(el => el.value === setLocale).text }}
        </p>
        <template v-if="setLocale === 'Ссылка'" class="link_load">
          <v-text-field v-model="link"
                        append-icon=""
                        class="hidden-sm-and-down"
                        dense
                        height="1.5em"
                        outlined
                        @input.once="linkStart = false"
          >
            <v-icon v-if="!$v.link.$invalid && !linkStart" slot="append" color="blue">
              mdi-check-circle
            </v-icon>
            <v-icon v-else-if="!linkStart" slot="append" color="red">
              mdi-minus-circle
            </v-icon>

          </v-text-field>
        </template>

        <template v-if="setLocale === 'Файл'">
          <v-container fluid class="file_load" v-model="file">
            <v-row no-gutters>
              <v-col>
                <div class="file_input_title_list">
                  Файлы .doc , .docx , .pdf .word , .png , .jpeg
                </div>
              </v-col>
              <v-col>
                <v-file-input

                    dense
                    outlined
                    prepend-icon=""
                    prepend-inner-icon="mdi-paperclip"
                ></v-file-input>
              </v-col>
            </v-row>

          </v-container>


        </template>

        <template  v-if="setLocale === 'iFrame'">
          <v-container fluid class="iFrame_load">
            <v-row>
              <v-col class="pt-0">
                <v-textarea
                    outlined
                    rows="10"
                    row-height="10"
                    auto-grow
                    height="303"
                    v-model="code"
                    @input.once="codeStart = false"

                    :class="{'wrong' : $v.code.$invalid}"

                >
                  <v-icon v-if="!$v.code.$invalid && !codeStart" slot="append" color="blue">
                    mdi-check-circle
                  </v-icon>
                  <v-icon v-else-if="!codeStart" slot="append"  color="red">
                    mdi-minus-circle
                  </v-icon>



                </v-textarea>
              </v-col>

            </v-row>

          </v-container>

        </template>
      </div>

    </v-row>
    <v-row class="mt-5">



      <v-btn
          depressed
          color="primary"
          class="text-capitalize"
          :disabled = "$v.code.$invalid && $v.link.$invalid"
          @click="sendData"
       >
        Сохранить
      </v-btn>

    </v-row>
  </v-container>

</template>
<script>
import {required, url, minLength, maxLength} from 'vuelidate/lib/validators'

export default {
  data: () => ({
    link: '',
    code: '',
    file: '',
    codeStart: true,
    linkStart: true,

    items: [
      {
        id: 0,
        value: 'Ссылка',
        icon: 'mdi-link',
        text: 'Вставить ссылку',

      },
      {
        id: 1,
        value: 'Файл',
        icon: ' mdi-file-document-outline',
        text: 'Загрузите файл'
      },
      {
        id: 2,
        value: 'iFrame',
        icon: 'mdi-monitor',
        text: 'Вставить code</>'
      },

    ],
    setLocale: 'iFrame'

  }),
  validations: {
    link: {
      url,
      required
    },
    code: {
      required,
      minLength: minLength(10),
      maxLayers: maxLength(2000)
    },
  },
  methods: {
    sendData:  function () {
      console.log(this.setLocale);
      let data = ''
      switch (this.setLocale){
        case "iFrame":{
          data = {
            type: 'iFrame',
            data: this.code
          }
          break;
        }
        case "Ссылка":{
          data = {
            type: 'link',
            data: this.link
          }
          break;
        }
        case "Файл":{
          data = {
            type: 'file',
            data: this.file
          }
          break;
        }
      }
      console.log(data)

    }
  }


}
</script>
<style scoped>
.root {
  display: flex;
  justify-content: center;
}
.select_wrap{
  margin-right: 10px;
}
p.title {
  font-size: 14px;
  font-family: 'Rubik', sans-serif;

}


.file_load {
  width: 364px;
  height: 74px;
  border: 1px dotted #A5B1BF;
  border-radius: 4px;
}

.file_input_title_list {
  overflow: hidden;
  font-size: 12px;
  color: #7F8FA4;
}
.iFrame_load{
  height: 303px;
  width: 364px;
  border-radius: 6px;

}

.loader_wrap{
  width: 364px;
}
.container.iFrame_load{
  padding: 0px !important;
}
.button_row{
  display: flex;
  justify-content: center;
}


</style>

