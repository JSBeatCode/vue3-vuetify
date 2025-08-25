<template>
  <v-app>
    <!-- App Bar -->
    <v-app-bar
      app
      color="primary"
      dark
      elevation="4"
    >
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>Vue3 + Vite + Vuetify</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn icon @click="toggleTheme">
        <v-icon>{{ $vuetify.theme.global.name === 'light' ? 'mdi-weather-night' : 'mdi-weather-sunny' }}</v-icon>
      </v-btn>
    </v-app-bar>

    <!-- Navigation Drawer -->
    <v-navigation-drawer
      v-model="drawer"
      app
      temporary
    >
      <v-list>
        <v-list-item
          prepend-icon="mdi-home"
          title="홈"
          value="home"
          @click="currentView = 'home'"
        ></v-list-item>
        <v-list-item
          prepend-icon="mdi-account"
          title="프로필"
          value="profile"
          @click="currentView = 'profile'"
        ></v-list-item>
        <v-list-item
          prepend-icon="mdi-cog"
          title="설정"
          value="settings"
          @click="currentView = 'settings'"
        ></v-list-item>
      </v-list>
    </v-navigation-drawer>

    <!-- Main Content -->
    <v-main>
      <v-container>
        <!-- Home View -->
        <div v-if="currentView === 'home'">
          <v-row>
            <v-col cols="12">
              <v-card elevation="3">
                <v-card-title class="text-h4 pa-6">
                  Vue3 + Vite + Vuetify 샘플
                </v-card-title>
                <v-card-text class="pa-6">
                  <p class="text-body-1 mb-4">
                    이것은 Vue 3, Vite, Vuetify를 사용한 샘플 애플리케이션입니다.
                  </p>
                  <v-row>
                    <v-col cols="12" md="4">
                      <v-card color="primary" variant="tonal">
                        <v-card-title>Vue 3</v-card-title>
                        <v-card-text>Options API와 함께하는 Vue</v-card-text>
                      </v-card>
                    </v-col>
                    <v-col cols="12" md="4">
                      <v-card color="secondary" variant="tonal">
                        <v-card-title>Vite</v-card-title>
                        <v-card-text>빠른 빌드 도구</v-card-text>
                      </v-card>
                    </v-col>
                    <v-col cols="12" md="4">
                      <v-card color="accent" variant="tonal">
                        <v-card-title>Vuetify</v-card-title>
                        <v-card-text>Material Design 컴포넌트</v-card-text>
                      </v-card>
                    </v-col>
                  </v-row>
                </v-card-text>
              </v-card>
            </v-col>
          </v-row>

          <!-- Form Example -->
          <v-row class="mt-4">
            <v-col cols="12" md="6">
              <v-card>
                <v-card-title>폼 예제</v-card-title>
                <v-card-text>
                  <v-form ref="form" v-model="formValid">
                    <v-text-field
                      v-model="formData.name"
                      :rules="nameRules"
                      label="이름"
                      required
                      prepend-icon="mdi-account"
                    ></v-text-field>
                    <v-text-field
                      v-model="formData.email"
                      :rules="emailRules"
                      label="이메일"
                      required
                      prepend-icon="mdi-email"
                    ></v-text-field>
                    <v-textarea
                      v-model="formData.message"
                      label="메시지"
                      rows="3"
                      prepend-icon="mdi-message"
                    ></v-textarea>
                    <v-btn
                      :disabled="!formValid"
                      color="primary"
                      @click="submitForm"
                    >
                      제출
                    </v-btn>
                  </v-form>
                </v-card-text>
              </v-card>
            </v-col>
            <v-col cols="12" md="6">
              <v-card>
                <v-card-title>액션 버튼들</v-card-title>
                <v-card-text>
                  <v-btn-group variant="outlined" class="mb-4">
                    <v-btn prepend-icon="mdi-heart" color="red">좋아요</v-btn>
                    <v-btn prepend-icon="mdi-share" color="blue">공유</v-btn>
                    <v-btn prepend-icon="mdi-bookmark" color="green">저장</v-btn>
                  </v-btn-group>
                  
                  <v-progress-linear
                    v-model="progress"
                    color="primary"
                    height="10"
                    class="mb-4"
                  ></v-progress-linear>
                  
                  <v-btn
                    @click="startProgress"
                    color="primary"
                    variant="outlined"
                    block
                  >
                    진행률 시작
                  </v-btn>
                </v-card-text>
              </v-card>
            </v-col>
          </v-row>
        </div>

        <!-- Profile View -->
        <div v-else-if="currentView === 'profile'">
          <v-card>
            <v-card-title>프로필</v-card-title>
            <v-card-text>
              <v-avatar size="100" class="mb-4">
                <v-icon size="60">mdi-account-circle</v-icon>
              </v-avatar>
              <h3>사용자 이름</h3>
              <p>이것은 프로필 페이지입니다.</p>
            </v-card-text>
          </v-card>
        </div>

        <!-- Settings View -->
        <div v-else-if="currentView === 'settings'">
          <v-card>
            <v-card-title>설정</v-card-title>
            <v-card-text>
              <v-switch
                v-model="settings.notifications"
                label="알림 허용"
                color="primary"
              ></v-switch>
              <v-switch
                v-model="settings.darkMode"
                label="다크 모드"
                color="primary"
                @change="toggleTheme"
              ></v-switch>
              <v-slider
                v-model="settings.volume"
                label="볼륨"
                min="0"
                max="100"
                thumb-label
              ></v-slider>
            </v-card-text>
          </v-card>
        </div>
      </v-container>
    </v-main>

    <!-- Footer -->
    <v-footer app color="primary" dark>
      <v-spacer></v-spacer>
      <span>&copy; 2024 Vue3 + Vite + Vuetify Sample</span>
      <v-spacer></v-spacer>
    </v-footer>

    <!-- Snackbar -->
    <v-snackbar
      v-model="snackbar.show"
      :color="snackbar.color"
      :timeout="3000"
    >
      {{ snackbar.text }}
      <template v-slot:actions>
        <v-btn
          variant="text"
          @click="snackbar.show = false"
        >
          닫기
        </v-btn>
      </template>
    </v-snackbar>
  </v-app>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      // Navigation
      drawer: false,
      currentView: 'home',
      
      // Form
      formValid: false,
      formData: {
        name: '',
        email: '',
        message: ''
      },
      
      // Progress
      progress: 0,
      
      // Settings
      settings: {
        notifications: true,
        darkMode: false,
        volume: 50
      },
      
      // Snackbar
      snackbar: {
        show: false,
        text: '',
        color: 'success'
      }
    }
  },
  computed: {
    nameRules() {
      return [
        v => !!v || '이름을 입력해주세요',
        v => (v && v.length <= 10) || '이름은 10자 이하여야 합니다'
      ]
    },
    emailRules() {
      return [
        v => !!v || '이메일을 입력해주세요',
        v => /.+@.+\..+/.test(v) || '유효한 이메일을 입력해주세요'
      ]
    }
  },
  methods: {
    toggleTheme() {
      this.$vuetify.theme.global.name = 
        this.$vuetify.theme.global.name === 'light' ? 'dark' : 'light'
      this.settings.darkMode = this.$vuetify.theme.global.name === 'dark'
    },
    submitForm() {
      if (this.formValid) {
        this.snackbar.text = '폼이 성공적으로 제출되었습니다!'
        this.snackbar.color = 'success'
        this.snackbar.show = true
        
        // Reset form
        this.formData.name = ''
        this.formData.email = ''
        this.formData.message = ''
      }
    },
    startProgress() {
      this.progress = 0
      const interval = setInterval(() => {
        if (this.progress >= 100) {
          clearInterval(interval)
          this.snackbar.text = '작업이 완료되었습니다!'
          this.snackbar.color = 'success'
          this.snackbar.show = true
        } else {
          this.progress += 10
        }
      }, 200)
    }
  }
}
</script>
