<template>
    <v-card
        class="my-10"
        :elevation="formElevation"
        :rounded="formRounded"
        width="100%"
    >

    <v-container
        class="px-10 text-center"
    >

            <v-form
                ref="form"
            >
                <v-text-field
                     label="Youtube動画ID"
                    v-model="movieUrl"
                    :error-messages="movieErrorMessages"
                    placeholder="動画URLの【v= 】の後に続く英数字"
                    style="width: 300px"
                />
                <v-text-field
                    ladel="コメント"
                    v-model="comment"
                    :rules="commentRules"
                    placeholder="動画紹介のための任意のコメント"
                    class="mb-5"
                    style="width:500px"
                />
                <v-btn
                    color="blue"
                    @click="formClick"
                    type="button"
                    x-large
                >
                CLICK!
                </v-btn>
            </v-form>
        </v-container>
    </v-card>
</template>

 <script>
        export default {
            name: 'InputForm',

            data() {
                return {
                    movieUrl: "",
                    movieErrorMessages: [],
                    comment: "",
                    commentRules: [
                        value => !!value || '入力してください', 
                        value => value.length <= 16 || '16文字以内で入力してください'
                    ]
                };
            },

            props: {
                formRounded: {
                    type: String,
                    required: false,
                    default: 'sm',
                },
                formElevation: {
                    type: String,
                    required: false,
                    default: '1',
                },
            },

            watch: {
                movieUrl: function (value) {
                    this.validateMovieUrl(value)
                }
            },

            methods: {
                formClick () {
                    this.validateMovieUrl(this.movieUrl)
                    const validateComment = this.$refs.form.validate()
                    if(this.movieErrorMessages.length === 0 && validateComment) {
                        this.$emit('storeMovie', this.movieUrl, this.comment)
                    }
                },

                validateMovieUrl (value) {
                    const stringValidation = /^[a-zA-Z0-9!-/:-@¥[-`{-~]+$/.test(value)
                    if (!value) {
                        this.movieErrorMessarges = [
                         '入力して下さい'
                        ] 
                    } else if (value.length !==11) {
                        this.movieErrorMessarges = [
                         '11文字で入力してください',
                        ]
                    } else if (!stringValidation) {
                        this.movieErrorMessarges = [
                        '半角英数字記号で入力してください',
                        ]
                    } else {
                        this.movieErrorMessarges = []
                    }
                },
            },
        }

</script>