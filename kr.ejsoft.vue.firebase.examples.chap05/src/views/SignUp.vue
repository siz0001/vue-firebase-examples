<template>
  <v-container
    class="fill-height"
    fluid
  >
    <v-row
      align="center"
      justify="center"
    >
      <v-col
        cols="12"
        sm="8"
        md="4"
      >
        <v-card class="elevation-12">
          <v-toolbar
            color="primary"
            dark
            flat
          >
            <v-toolbar-title>SignUp</v-toolbar-title>
            <v-spacer></v-spacer>
          </v-toolbar>
          <v-card-text>
            <v-form>
              <v-text-field
                label="Email"
                name="email"
                prepend-icon="mdi-account"
                type="text"
                v-model="email"
              ></v-text-field>
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-btn color="secondary" @click="$router.push('/signin')">SignIn</v-btn>
            <v-spacer></v-spacer>
            <v-btn color="primary" @click="signup">SignUp</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import firebase from 'firebase';

export default {
  data: () => ({
    email : '',
  }),
  created: () => {
      const protocol = location.protocol;
      const hostName = location.hostname;
      const port = location.port;

      let url = protocol + '//' + hostName + (port ? ':' + port : '');
      url += '/#/signupfinish';

      console.log(url);
  },
  methods: {
    signup() {
      console.log('Signup', this.email);

      if(!this.email) {
        alert('전자우편을 입력하여 주십시오.');
        return;
      }
      
      const protocol = location.protocol;
      const hostName = location.hostname;
      const port = location.port;

      let url = protocol + '//' + hostName + (port ? ':' + port : '');
      // url += '/#/signupfinish';
      url += '/callback/email';

      console.log(url);
      const actionCodeSettings = {
        // URL you want to redirect back to. The domain (www.example.com) for this
        // URL must be whitelisted in the Firebase Console.
        // url: 'https://www.example.com/finishSignUp?cartId=1234',
        url: url,
        // This must be true.
        handleCodeInApp: true,
        // iOS: {
        //   bundleId: 'com.example.ios'
        // },
        // android: {
        //   packageName: 'com.example.android',
        //   installApp: true,
        //   minimumVersion: '12',
        // },
        // dynamicLinkDomain: 'example.page.link',
      };

      const _this = this;
      firebase.auth().sendSignInLinkToEmail(this.email, actionCodeSettings)
      .then(function() {
        // The link was successfully sent. Inform the user.
        // Save the email locally so you don't need to ask the user for it again
        // if they open the link on the same device.
        window.localStorage.setItem('emailForSignIn', _this.email);
      })
      .catch(function(error) {
        // Some error occurred, you can inspect the code: error.code
        console.log(error)
        // if(error.code === "auth/invalid-email") {}
        if(error && error.message) {
          alert(error.message);
        }
      });

    }
  }
};
</script>