<template>
  <v-container class="container" fluid>
    <v-form>
      <v-row class="searchBoxContainer">
        <v-col class="col-xs-6">
          <v-row class="searchBox" color="primary">
            <ul>
              <li :class="{ active: isPC }" @click="toggleActive('origin')">
                <img src="../assets/iconfinder_32-windows8_104472.png" class="logo"/>
              </li>
              <li :class="{ active: isPS4 }" @click="toggleActive('psn')">
                <img src="../assets/game.png" class="logo"/>              
              </li>
              <li :class="{ active: isXbox }" @click="toggleActive('xbl')">
                <img src="../assets/xbox.png" class="logo"/>
              </li>
            </ul>
          <v-text-field class="text-input" v-model="userName" label="Search by Gamer Name">
          </v-text-field>
          <v-btn color="primary" @click="getUserData">Search</v-btn>
          </v-row>
        </v-col>
      </v-row>
      
      <v-row>
        <v-col>
          <img class="profilePicture" :src="this.platformInfo.avatarUrl"/>
        </v-col>
        <v-col class="profileInfo">
          <p>{{platformInfo.platformUserId}}</p>
        </v-col>
      </v-row>
      <p>{{platformInfo}}</p>
      <p>{{userInfo}}</p>
      <p>{{current}}</p>
    </v-form>
  </v-container>
</template>

<script>
import axios from 'axios';

export default {
  name: 'ApexTracker',
  props: {
    msg: String
  },
  data() {
    return {
      userName: '',
      platform: 'origin',
      data: {},
      platformInfo: {},
      userInfo: {},
      current: {
        currentSeason: '',
        activeLegend: '',
        activeLegendName: ''
      },
      isPC: true,
      isXbox: false,
      isPS4: false,
    }
  },
  methods: {
    getUserData() {
      axios.get(`https://api.tracker.gg/api/v2/apex/standard/profile/${this.platform}/${this.userName}`, {
        headers: {
          "TRN-Api-Key": 'acf80b73-e025-422a-9a5c-59f64e229f33',
          "Access-Control-Allow-Origin": '*',
          "Accept": 'application/json',
        }
      })
        .then((response) => {
          this.data = response.data;
          this.setData(this.data);
        }, (error) => {
          console.log(error);
        });
    },
    toggleActive(platform) {
      if (platform === 'origin') {
        this.isPC = true;
        this.isXbox = false;
        this.isPS4 = false;
        this.platform = platform;
      } else if (platform === 'xbl') {
        this.isPC = false;
        this.isXbox = true;
        this.isPS4 = false;
        this.platform = platform;        
      } else if (platform === 'psn') {
        this.isPC = false;
        this.isXbox = false;
        this.isPS4 = true;
        this.platform = platform;
      }
    },
    setPlatformInfo(response) {
      this.platformInfo = response.data.platformInfo;
    },
    setUserInfo(response) {
      this.userInfo = response.data.userInfo;
    },
    setCurrent(response) {
      this.current = response.data.metadata;
    },
    setData(response) {
      this.setPlatformInfo(response);
      this.setUserInfo(response);
      this.setCurrent(response);
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
li {
  display: inline-block;
  padding: 10px;
  background-color: grey;
}

.v-application ul {
  padding: 0px;
}

.active {
  background-color: white;
}

.logo {
  height: 40px;
  width: 40px;
}

.text-input {
  margin: 10px;
  font-size: 20px;
}

.searchBox {
  border: 3px solid;
  border-radius: 20px;
  padding: 15px;
  margin: 10px;
  display: flex;
  align-items: center;
  flex-grow: 2;
}

.searchBoxContainer {
  padding: 15px 100px 15px 100px;
}

.container {
  padding: 0px 80px 0px 80px;
}

.profilePicture {
  width: 150px;
  height: 150px;
}

.profileInfo {
  align-items: flex-start;
}
</style>
