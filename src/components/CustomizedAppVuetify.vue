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
        <v-icon>{{ theme.global.name.value === 'light' ? 'mdi-weather-night' : 'mdi-weather-sunny' }}</v-icon>
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
                  Vue3 + Vite + Vuetify 샘플 (Composition API)
                </v-card-title>
                <v-card-text class="pa-6">
                  <p class="text-body-1 mb-4">
                    이것은 Vue 3, Vite, Vuetify를 사용한 샘플 애플리케이션입니다.
                  </p>
                  <v-row>
                    <v-col cols="12" md="4">
                      <v-card color="primary" variant="tonal">
                        <v-card-title>Vue 3</v-card-title>
                        <v-card-text>Composition API와 함께하는 최신 Vue</v-card-text>
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
                    
                    <!-- 커스터마이징된 v-combobox -->
                    <v-combobox
                      v-model="formData.skills"
                      :items="skillOptions"
                      :rules="skillRules"
                      label="기술 스택"
                      hint="여러 기술을 선택하거나 새로 추가할 수 있습니다"
                      persistent-hint
                      chips
                      closable-chips
                      multiple
                      prepend-icon="mdi-code-tags"
                      color="primary"
                      variant="outlined"
                      :filter-keys="['title', 'desc']"
                      item-title="title"
                      item-value="value"
                      return-object
                      :loading="skillsLoading"
                      @update:search="onSkillSearch"
                      @update:model-value="onSkillsChange"
                    >
                      <!-- 커스텀 item 슬롯 -->
                      <template v-slot:item="{ props, item }">
                        <v-list-item
                          v-bind="props"
                          :prepend-avatar="item.raw.icon"
                          :subtitle="item.raw.desc"
                        >
                          <template v-slot:append>
                            <v-chip
                              :color="item.raw.level === 'Expert' ? 'success' : item.raw.level === 'Intermediate' ? 'warning' : 'info'"
                              size="small"
                            >
                              {{ item.raw.level }}
                            </v-chip>
                          </template>
                        </v-list-item>
                      </template>
                      
                      <!-- 커스텀 chip 슬롯 -->
                      <template v-slot:chip="{ props, item }">
                        <v-chip
                          v-bind="props"
                          :prepend-icon="item.raw?.icon || 'mdi-code-tags'"
                          :color="getChipColor(item.raw?.level)"
                          closable
                          size="small"
                        >
                          {{ item.raw?.title || item }}
                        </v-chip>
                      </template>
                      
                      <!-- 선택사항 없을 때 -->
                      <template v-slot:no-data>
                        <v-list-item>
                          <v-list-item-title>
                            "{{ skillSearchText }}"를 새 기술로 추가하려면 Enter를 누르세요
                          </v-list-item-title>
                        </v-list-item>
                      </template>
                    </v-combobox>
                    
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
              
              <!-- 선택된 스킬 표시 -->
              <v-card class="mt-4" v-if="formData.skills.length > 0">
                <v-card-title>선택된 기술 스택</v-card-title>
                <v-card-text>
                  <div class="d-flex flex-wrap ga-2">
                    <v-chip
                      v-for="skill in formData.skills"
                      :key="skill.value || skill"
                      :prepend-icon="skill.icon || 'mdi-code-tags'"
                      :color="getChipColor(skill.level)"
                      variant="elevated"
                    >
                      {{ skill.title || skill }}
                      <template v-if="skill.level">
                        <v-divider vertical class="mx-2"></v-divider>
                        <span class="text-caption">{{ skill.level }}</span>
                      </template>
                    </v-chip>
                  </div>
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

<script setup>
import { ref, reactive, computed, watch } from 'vue'
import { useTheme } from 'vuetify'

const theme = useTheme()

// Navigation
const drawer = ref(false)
const currentView = ref('home')

// Form
const form = ref()
const formValid = ref(false)
const formData = reactive({
  name: '',
  email: '',
  message: '',
  skills: []
})

// Skills combobox data
const skillsLoading = ref(false)
const skillSearchText = ref('')

const skillOptions = ref([
  {
    title: 'Vue.js',
    value: 'vue',
    desc: '프로그레시브 JavaScript 프레임워크',
    icon: 'mdi-vuejs',
    level: 'Expert'
  },
  {
    title: 'React',
    value: 'react',
    desc: 'UI 구축을 위한 JavaScript 라이브러리',
    icon: 'mdi-react',
    level: 'Intermediate'
  },
  {
    title: 'Node.js',
    value: 'nodejs',
    desc: 'JavaScript 런타임 환경',
    icon: 'mdi-nodejs',
    level: 'Intermediate'
  },
  {
    title: 'Python',
    value: 'python',
    desc: '범용 프로그래밍 언어',
    icon: 'mdi-language-python',
    level: 'Expert'
  },
  {
    title: 'TypeScript',
    value: 'typescript',
    desc: 'JavaScript의 타입 안전성을 높인 언어',
    icon: 'mdi-language-typescript',
    level: 'Beginner'
  },
  {
    title: 'Docker',
    value: 'docker',
    desc: '컨테이너화 플랫폼',
    icon: 'mdi-docker',
    level: 'Intermediate'
  }
])

// Validation rules
const nameRules = [
  v => !!v || '이름을 입력해주세요',
  v => (v && v.length <= 10) || '이름은 10자 이하여야 합니다'
]

const emailRules = [
  v => !!v || '이메일을 입력해주세요',
  v => /.+@.+\..+/.test(v) || '유효한 이메일을 입력해주세요'
]

const skillRules = [
  v => v.length > 0 || '최소 하나의 기술을 선택해주세요'
]

// Progress
const progress = ref(0)

// Settings
const settings = reactive({
  notifications: true,
  darkMode: false,
  volume: 50
})

// Snackbar
const snackbar = reactive({
  show: false,
  text: '',
  color: 'success'
})

// Computed
const getChipColor = computed(() => {
  return (level) => {
    switch(level) {
      case 'Expert': return 'success'
      case 'Intermediate': return 'warning'
      case 'Beginner': return 'info'
      default: return 'primary'
    }
  }
})

// Methods
const toggleTheme = () => {
  theme.global.name.value = theme.global.current.value.dark ? 'light' : 'dark'
  settings.darkMode = theme.global.current.value.dark
}

const submitForm = () => {
  if (formValid.value) {
    snackbar.text = `폼이 성공적으로 제출되었습니다! 선택된 기술: ${formData.skills.map(s => s.title || s).join(', ')}`
    snackbar.color = 'success'
    snackbar.show = true
    
    console.log('제출된 데이터:', formData)
  }
}

const startProgress = () => {
  progress.value = 0
  const interval = setInterval(() => {
    if (progress.value >= 100) {
      clearInterval(interval)
      snackbar.text = '작업이 완료되었습니다!'
      snackbar.color = 'success'
      snackbar.show = true
    } else {
      progress.value += 10
    }
  }, 200)
}

// Combobox 이벤트 핸들러
const onSkillSearch = (searchText) => {
  skillSearchText.value = searchText
  // 실제 앱에서는 여기서 API 호출 등을 할 수 있습니다
  if (searchText && searchText.length > 2) {
    skillsLoading.value = true
    // 시뮬레이션된 검색 로딩
    setTimeout(() => {
       alert('검색 완료')
      skillsLoading.value = false
    }, 500)
  }
}

const onSkillsChange = (newSkills) => {
  // 새로 추가된 문자열 항목을 객체로 변환
  const processedSkills = newSkills.map(skill => {
    if (typeof skill === 'string') {
      return {
        title: skill,
        value: skill.toLowerCase().replace(/\s+/g, '-'),
        desc: '사용자 추가 기술',
        icon: 'mdi-code-tags',
        level: 'Custom'
      }
    }
    return skill
  })
  
  formData.skills = processedSkills
  
  snackbar.text = `기술 스택이 업데이트되었습니다: ${processedSkills.length}개`
  snackbar.color = 'info'
  snackbar.show = true
}

// Watch for skills changes
watch(() => formData.skills, (newSkills) => {
  console.log('선택된 기술들:', newSkills)
}, { deep: true })
</script>