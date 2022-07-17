<template>
    <section class="main">
        <div class="card-action">
            <button class="card-btn"
                    :class="{'card-btn_active': isActiveSwitcherItalic}"
                    @click="switchItalic"
            >
                Курсив
            </button>
            <button class="card-btn"
                    @click="addCardFromCardsStorage"
            >
                Добавить еще
            </button>
        </div>
        <div class="card-group">
            <div v-for="card in cards"
                 :key="card.id"
                 class="card"
            >
                <editor :api-key="editorApiKey"
                        :id="`card_title_${card.id}`"
                        class="card-title"
                        :initial-value="card.title"
                        :init="editorConfig"
                        @selectionChange="handlerSelectionChange"
                        @focus="handlerFocus(`card_title_${card.id}`)"
                        @blur="handlerBlur"
                />
                <img class="card-img"
                     :src="card.urlForPhoto"
                >
                <editor :api-key="editorApiKey"
                        :id="`card_body_${card.id}`"
                        class="card-body"
                        :initial-value="card.body"
                        :init="editorConfig"
                        @selectionChange="handlerSelectionChange"
                        @focus="handlerFocus(`card_body_${card.id}`)"
                        @blur="handlerBlur"
                />
            </div>
        </div>
    </section>
</template>

<script>
  import Editor from '@tinymce/tinymce-vue'
  import axios from 'axios'
  import tinymce from 'tinymce'
  import 'tinymce/themes/silver/theme.min'
  import 'tinymce/icons/default/icons.min'
  import 'tinymce/skins/ui/oxide/skin.min.css'
  import 'tinymce/skins/ui/oxide/content.inline.min.css'

  export default {
      name: 'TZ',
      components: {
          Editor
      },
      props: {
          msg: String
      },
      data() {
          return {
              isActiveSwitcherItalic: false,
              selectionNode: null,
              cardsStorage: null,
              cards: null,
              editorApiKey: '24ccjtk66ut40q7939e8v3yhp0wed23eliprvjih6uzle3oq',
              editorConfig: {
                  menubar: false,
                  toolbar: false,
                  inline: true
              }
          }
      },
      created() {
          axios.get('https://jsonplaceholder.typicode.com/posts').then(posts => {
              this.cardsStorage = posts.data

              axios.get('https://jsonplaceholder.typicode.com/photos?albumId=1&albumId=2').then(photos => {
                  photos.data.forEach((elem, index) => {
                      this.cardsStorage[index].urlForPhoto = elem.url
                  })

                  this.cards = this.cardsStorage.slice(0, 3)
              })
          })
      },
      methods: {
          switchItalic() {
              if (this.selectionNode) {
                  let formatter = this.selectionNode.formatter
                  formatter.toggle('italic', this.selectionNode.selection.getRng())
              }
          },
          addCardFromCardsStorage() {
              this.cards.push(this.cardsStorage[this.cards.length])
          },
          handlerSelectionChange() {
              this.isActiveSwitcherItalic = this.selectionNode.formatter.match(
                  'italic',
                  this.selectionNode.selection.getRng()
              )
          },
          handlerFocus(nodeId) {
              this.selectionNode = tinymce.get(nodeId)
          },
          handlerBlur() {
              this.isActiveSwitcherItalic = false
          }
      }
  }
</script>

<style scoped>
    .main {
        display: flex;
        flex-flow: row nowrap;
        justify-content: center;;
        margin: 50px 0;
    }

    .card-group {
        display: flex;
        flex-flow: column wrap;
        width: 500px;
    }

    .card {
        margin-bottom: 50px;
        box-shadow: 0 17px 42px rgb(0 0 0 / 15%);
        padding: 25px;
    }

    .card-title {
        font-size: 24px;
        margin-bottom: 25px;
    }

    .card-img {
        max-width: 100%;
        margin-bottom: 25px;
        max-height: 200px;
        width: 100%;
        object-fit: cover;
    }

    .card-body {
        font-size: 18px;
    }

    .card-action {
        width: 300px;
    }

    .card-btn {
        background: #fff;
        color: #bbb;
        border: 1px solid #bbbbbb;
        border-radius: 4px;
        padding: 5px 10px;
        cursor: pointer;
    }

    .card-btn:hover, .card-btn_active {
        background: #bbb;
        color: #fff;
    }
</style>
