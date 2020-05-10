<template>
  <div>

    <div class="container">
      <div class="row justify-content-between">
        <div class="col-12 col-md-auto text-left">
          <div>Crear usuario</div>
          <div class="row">
            <el-form class="" :model="userForm" :rules="messageFormRules" ref="userForm">
              <div class="row">
        
                <div class="col">
                  <el-form-item prop="userID">
                    <el-input v-model="userForm.userID" placeholder="ID Usuario"></el-input>
                  </el-form-item>
                </div>
        
                <div class="col">
                  <el-form-item prop="userName">
                    <el-input v-model.number="userForm.userName" placeholder="Nombre"></el-input>
                  </el-form-item>
                </div>

                <div class="col">
                  <el-form-item>
                    <el-button-group>
                      <el-button type="success" @click="submitForm('userForm')">Crear</el-button>
                    </el-button-group>
                  </el-form-item>
                </div>
        
              </div>
            </el-form>
          </div>
        </div>

        <div class="col-12 col-md-auto text-left">
          <div>Añadir usuario al chat actual</div>
          <div class="row">
            <el-form class="" :model="userAddToChatForm" :rules="messageFormRules" ref="userAddToChatForm">
              <div class="row">
        
                <div class="col">
                  <el-form-item prop="userID">
                    <el-input v-model="userAddToChatForm.userID" placeholder="ID Usuario"></el-input>
                  </el-form-item>
                </div>

                <div class="col">
                  <el-form-item>
                    <el-button-group>
                      <el-button type="success" @click="submitForm('userAddToChatForm')">Crear</el-button>
                    </el-button-group>
                  </el-form-item>
                </div>
        
              </div>
            </el-form>
          </div>
        </div>

        <div class="col-12 col-md-auto text-left">
          <div>Crear chat</div>
          <el-form class="" :model="chatForm" :rules="messageFormRules" ref="chatForm">
            <div class="row">
      
              <div class="col">
                <el-form-item prop="chatName">
                  <el-input v-model="chatForm.chatName" placeholder="Chat name"></el-input>
                </el-form-item>
              </div>

              <div class="col">
                <el-form-item>
                  <el-button-group>
                    <el-button type="success" @click="submitForm('chatForm')">Crear</el-button>
                  </el-button-group>
                </el-form-item>
              </div>
      
            </div>
          </el-form>
        </div>
      </div>

      <div class="row">
        <div class="col-12 col-md-12 text-left">
          <div class="my-2">Enviar mensaje</div>
          <div class="row">
            <el-form class="w-100" :model="messageForm" :rules="messageFormRules" ref="messageForm">
              <div class="row">
        
                <div class="col">
                  <el-form-item prop="userID">
                    <el-input v-model="messageForm.userID" placeholder="ID Usuario"></el-input>
                  </el-form-item>
                </div>
        
                <div class="col-md-6">
                  <el-form-item prop="message">
                    <el-input v-model="messageForm.message" placeholder="Mensaje"></el-input>
                  </el-form-item>
                </div>

                <div class="col">
                  <el-form-item>
                    <el-button-group>
                      <el-button type="primary" @click="selectFile">Imagen</el-button>
                      <el-button type="success" @click="submitForm('messageForm')">Enviar</el-button>
                    </el-button-group>
                  </el-form-item>
                </div>
        
              </div>
            </el-form>
          </div>
        </div>
      </div>
    </div>

    <!-- <div style="displa: flex" class="">
      <input type="text" placeholder="ID Cliente" v-model="clientID" style="margin-right: 1rem;">

      <input type="text" placeholder="message" v-model="messageInput">
      <button @click="sendMessage">send</button>

      <button style="margin: 0 1rem;" @click="selectFile">File</button>
    </div> -->
    <input style="display: none" type="file" ref="chatFile" accept="image/*" @change="handleFile">

    <!-- parte bonitah -->

    Chat actual: {{actualChat.name}} {{actualChat.chatID}} <br>

    <div class="container">
      <div class="row chat_box mt-4">
        <div class="col-8 chat_box_left px-0">
          <div class="chat_box_msg-container">
            <div class="row mx-0 flex-nowrap" style="overflow-x: scroll;">
              <el-button type="primary" icon="el-icon-chat-round" class="mx-n1" @click="changeActualChat (chat)"
                v-for="(chat, index) in chats" :key="index">Chat {{chat.name}}</el-button>
            </div>

            <div class="msgs_board my-3">

              <div class="w-100 px-5" v-for="(message, index) in messages" :key="index">
              
                <div class="row mx-0 align-items-center">
                  <el-avatar :src="message.senderPic"></el-avatar>
                  <span class="mx-2">{{message.senderName}}</span>
                  <small class="text-muted small">at {{ moment(message.timestamp).format("YYYY/MM/d") }}</small>

                  <el-button class="ml-2" type="text" icon="el-icon-delete-solid" @click="deleteMessage(message.messageID)"></el-button>
                </div>
                <div class="text-left mx-0 mt-1">
                  <div>{{message.content}}</div>
                  <div v-if="message.hasFile">
                    <el-image :src="message.file.downloadURL"></el-image>
                    <!-- <img :src="message.file.downloadURL" alt=""> -->
                  </div>
                </div>
                <el-divider></el-divider>
  
              </div>

            </div>

          </div>
        </div>
        <div class="col-4 chat_box_right">
          <div class="row flex-column h-100">

            <!-- <span style="height: 3vh;">"Registered users"</span>
            <el-divider></el-divider>
            <div class="d-flex flex-grow-1">

              <div class="registered-users_box w-100">

                <div class="row px-4 mb-2" v-for="(item, index) in 2" :key="index">
                  <el-avatar :src="randomPic()"></el-avatar>
                  <span class="d-flex flex-grow-1 justify-content-left align-items-center ml-3">
                    <span>Name</span>
                    <span class="mx-3"> - </span>
                    <span>123</span>
                  </span>
                </div>
    
              </div>

            </div> -->

          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import H3Chat from 'h3-firebase-chat';
import firebase from 'firebase/app';
import 'firebase/database';
import moment from 'moment';

H3Chat({
    credentials: {
      apiKey: "AIzaSyCcSRwPdU0EoscZJeR83nFRmTsCNrxWtuk",
      authDomain: "qchao-4bb0f.firebaseapp.com",
      databaseURL: "https://qchao-4bb0f.firebaseio.com",
      projectId: "qchao-4bb0f",
      storageBucket: "qchao-4bb0f.appspot.com",
      messagingSenderId: "181613745010",
      appId: "1:181613745010:web:f20215c856fe29692803eb"
    }
});

export default {
  name: 'HelloWorld',
  data () {
    return {
      messages: [],
      messageInput: null,
      idChat: "-M5u5wm8pnfkEjdielk7",
      clientID: null,
      file: null,
      actualChat: {},
      chatForm: {},
      userForm: {},
      messageForm: {},
      userAddToChatForm: {},
      chats: [],
      messageFormRules: {
        message: [
          { required: true, message: 'Este campo es requerido', trigger: 'submit' },
          { max: 200, message: 'Tamaño maximo de mensaje 500 caracteres', trigger: 'change' }
        ],
        userID: [
          { required: true, message: 'Este campo es requerido', trigger: 'submit' },
        ],
        userName: [
          { required: true, message: 'Este campo es requerido', trigger: 'submit' },
          { max: 20, message: 'Tamaño maximo de 20 caracteres', trigger: 'change' }
        ],
        chatName: [
          { required: true, message: 'Este campo es requerido', trigger: 'submit' },
          { max: 20, message: 'Tamaño maximo de 20 caracteres', trigger: 'change' }
        ],
      }
    }
  },
  created () {
    H3Chat.getChatIDs().then((res) => {
      this.chats = res
      if (this.chats.length > 0) {
        this.actualChat = this.chats[0]
        H3Chat.getMessages(this.actualChat.chatID, (messages) => {
          this.messages = messages
        })
      }
    })
  },
  methods: {
    sendMessage () {
      let chatID = this.actualChat.chatID
      let messageData = {
          content: this.messageForm.message, // Actual text of the message
          hasFile: (this.file)? true:false, // Has file
          file: this.file, // File
          senderID: this.messageForm.userID,
      }

      firebase.database().ref("users")
      .orderByChild("id").equalTo(this.messageForm.userID).once('value').then(snapshot =>{
        let user = {}
        snapshot.forEach(childSnapshot => {
          user = childSnapshot.val(); 
          return
        });

        if (Object.keys(user).length === 0 && user.constructor === Object) { // user empty
          this.$notify({
            title: 'Usuario no existe',
            type: 'error',
            message: `El usuario no existe con ese id`
          });
          return
        }

        console.log("EL USER", user)
        messageData.senderName = user.name
        messageData.senderPic = user.picture

        H3Chat.sendMessage (chatID, messageData, true).catch(err => {
          this.$notify({
            title: 'Error al enviar el mensaje',
            type: 'error',
            message: `Error al enviar el mensaje ${err}`
          });
        })
      })
    },
    deleteMessage (messageID) {
      H3Chat.deleteMessage(this.actualChat.chatID, messageID).then(res => {
        this.$notify({
          title: 'Mensaje eliminado!',
          type: 'success',
          message: "El mensaje ha sido eliminado"
        });
      })
    },
    selectFile () {
      console.log("El ref", this.$refs.chatFile)
      this.$refs.chatFile.click()
    },
    handleFile (event) {
      let files = event.target.files
      this.file = files[0]
      console.log("File", files)
    },
    changeActualChat (chat) {
      this.actualChat = chat
      H3Chat.getMessages(chat.chatID, (messages) => {
        this.messages = messages
      })
    },
    randomPic () {
      return `https://picsum.photos/id/${Math.floor((Math.random() * 998) + 1)}/100/100`
    },
    createChat () {
      H3Chat.createChat({
        name: this.chatForm.chatName
      }).then(() => {
        alert("Chat created!")
      })
    },
    createUser () {
      let user = {
        id: this.userForm.userID,
        name: this.userForm.userName,
        picture: this.randomPic ()
      }
      firebase.database().ref("users").push(user)
    },
    addUserToChat () {
      H3Chat.addUserToChat(this.userAddToChatForm.userID, this.actualChat.chatID).then(() => {
        this.$notify({
          title: 'Usuario añadido al chat!',
          type: 'success',
          message: "El usuario ha sido añadido correctamente"
        });
      });
    },
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          if (formName == 'chatForm') {
            this.createChat()
          } else if (formName == 'userForm') {
            this.createUser()
          } else if (formName == 'messageForm') {
            this.sendMessage ();
          } else if (formName == 'userAddToChatForm') {
            this.addUserToChat ();
          }
          this.resetForm(formName)
        } else {
          console.log('error submit!!');
          return false;
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
    moment () {
      return moment();
    }
  },
  computed: {
  },
}
</script>

<style lang="scss">
  @import "bootstrap";
  .chat {
    &_box {
      border: 1px solid gray;
      height: 80vh;

      &_right {
        border-left: 1px solid gray;
      }

      &_left {
        border-right: 1px solid gray;
      }

      &_msg-container {
        max-height: 100%;
        overflow: hidden;
      }
    }
  }

  .msgs {
    &_board {
      max-height: calc(80vh - (3vh + 51px));
      overflow-x: hidden;
      overflow-y: scroll;
    }
  }

  .registered-users {
    &_box {
      max-height: calc(80vh - (3vh + 51px));
      overflow-y: scroll;
      overflow-x: hidden;
    }
  }
</style>