<template>
  <div class="container mt-4">
    <div class="col-sm-4 mx-auto">

      <h2 class="reg-title">Регистрация</h2>
      <form @submit.prevent="userRegister" novalidate>
        <div v-show="step === 1">

          <div v-if="regMessage" class="alert alert-success" role="alert">
             Вы успешно зарегистрировались!
          </div>

          <div class="form-group">
              <label for="last_name">Фамилия*</label>
              <input @blur="$v.formReg.last_name.$touch()"
                     :class="{'is-invalid reqInput': $v.formReg.last_name.$error}"
                     v-model.trim="formReg.last_name"
                     type="text" class="form-control" id="last_name" placeholder="Например, Иванов">

              <div class="invalid-feedback" v-if="!$v.formReg.last_name.required">{{ reqText }}</div>
              <div class="invalid-feedback" v-if="!$v.formReg.last_name.alpha">{{ alphaText }}</div>
          </div>

          <div class="form-group">
              <label for="first_name">Имя*</label>

              <input @blur="$v.formReg.first_name.$touch()"
                     :class="status($v.formReg.first_name)"
                     v-model.trim="formReg.first_name"
                     type="text" class="form-control" id="first_name" placeholder="Например, Иван">

              <div class="invalid-feedback" v-if="!$v.formReg.first_name.required">{{ reqText }}</div>
              <div class="invalid-feedback" v-if="!$v.formReg.first_name.alpha">{{ alphaText }}</div>
          </div>

          <div class="form-group">
              <label for="middle_name">Отчество</label>
              <input @blur="$v.formReg.middle_name.$touch()"
                     :class="status($v.formReg.middle_name)"
                     v-model.trim="formReg.middle_name"
                     type="text" class="form-control" id="middle_name" placeholder="Например, Иванович">
                     <div class="invalid-feedback" v-if="!$v.formReg.middle_name.alpha">{{ alphaText }}</div>
          </div>

          <div class="form-group">
                <label for="birthdate">Дата рождения*</label>
                <input @blur="$v.formReg.birthdate.$touch()"
                     :class="status($v.formReg.birthdate)"
                     v-model.trim="formReg.birthdate"                  
                     type="date" class="form-control" id="birthdate">
              <div class="invalid-feedback" v-if="!$v.formReg.birthdate.required">{{ reqText }}</div>
          </div>

          <div class="form-group">
                <label for="phone">Номер телефона*</label>
                <input @blur="$v.formReg.phone.$touch()"
                     :class="status($v.formReg.phone)"
                     v-model.trim="formReg.phone"                  
                     type="phone" class="form-control" id="phone" placeholder="Например, 78009002233">
              <div class="invalid-feedback" v-if="!$v.formReg.phone.required">{{ reqText }}</div>
              <div class="invalid-feedback" v-if="!$v.formReg.phone.numeric">{{ numText }}</div>
              <div class="invalid-feedback" v-if="!$v.formReg.phone.minLength">{{ minLengthText }}</div>
              <div class="invalid-feedback" v-if="!$v.formReg.phone.maxLength">{{ maxLengthText }}</div>
          </div>

          <div class="form-group">
               <div class="form-check">
                     <span>Пол</span><br>
                     <input class="check-input" type="radio" value="male" name="genderRadios" id="male" v-model="gendere">
                     <label class="check-label" for="male">
                      Мужчина
                     </label>
               </div>
               <div class="form-check">
                     <input class="check-input" type="radio" value="male" name="genderRadios" id="female" v-model="gendere">
                     <label class="check-label" for="female">
                      Женщина
                     </label>
               </div>
          </div>

          <div class="form-group">
                  <label for="favoriteClients">Группа клиентов*</label>
                  <select
                     v-model.trim="formReg.favoriteClients"
                     type="favoriteClients" class="form-control" id="favoriteClients" multiple>
                    <option 
                     v-for="(favoriteClients, index) in clients"
                     :value="favoriteClients.value"
                     :key="index"
                    >
                      {{ favoriteClients.label }}
                    </option>
                  </select>
                  <div class="invalid-feedback" v-if="!$v.formReg.favoriteClients.required">{{ reqText }}</div>
          </div>
          <div class="form-group">
                  <label for="attending_doctor">Лечащий Врач</label>
                  <select id="attending_doctor"  class="form-control" v-model="attending_doctor">
                    <option 
                     v-for="(attending_doctor, index) in doctors"
                     :value="attending_doctor.value"
                     :key="index"
                    >
                      {{ attending_doctor.label }}
                    </option>
                  </select>
                </div>              
                <div class="form-check">
                  <input type="checkbox" class="check-input" id="notification" v-model="agreeWithSendSms">
                  <label class="check-label" for="notification">Не отправлять смс</label>
          </div>                

          <button @click="step++" :disabled="disabledBtn1"
                  type="button" class="btn btn-primary">Следующий шаг</button>          
        </div>

        <transition name="slide-fade">
          <div v-show="step === 2">

        <div class="form-group">
          <label for="indInputField">Индекс</label>
                <input @blur="$v.formReg.indInputField.$touch()"
                     :class="status($v.formReg.indInputField)"
                     v-model.trim="formReg.indInputField"                  
                     type="ind" class="form-control" id="indInputField" placeholder="Например, 628500">
                     <div class="invalid-feedback" v-if="!$v.formReg.indInputField.numeric">{{ numText }}</div>
                  
                  
        </div>
        <div class="form-group">
                  <label for="country">Страна</label>
                  <input @blur="$v.formReg.country.$touch()"
                     :class="status($v.formReg.country)"
                     v-model.trim="formReg.country"
                     type="text" class="form-control" id="country" placeholder="Например, Россия">
                     <div class="invalid-feedback" v-if="!$v.formReg.country.alpha">{{ alphaText }}</div>
        </div>
        <div class="form-group">
                  <label for="region">Область</label>
                  <input @blur="$v.formReg.region.$touch()"
                     :class="status($v.formReg.region)"
                     v-model.trim="formReg.region"
                     type="text" class="form-control" id="region" placeholder="Например, Московская область">
        </div>
        <div class="form-group">
                  <label for="city">Город*</label>

                  <input @blur="$v.formReg.city.$touch()"
                         :class="status($v.formReg.city)"
                         v-model.trim="formReg.city"
                         type="text" class="form-control" id="city" placeholder="Например, Москва">

                  <div class="invalid-feedback" v-if="!$v.formReg.city.required">{{ reqText }}</div>
                  <div class="invalid-feedback" v-if="!$v.formReg.city.alpha">{{ alphaText }}</div>
        </div>
        <div class="form-group">
                  <label for="street">Улица</label>
                  <input @blur="$v.formReg.street.$touch()"
                     :class="status($v.formReg.street)"
                     v-model.trim="formReg.street"
                     type="text" class="form-control" id="street" placeholder="Например, Советская">
                     <div class="invalid-feedback" v-if="!$v.formReg.street.alpha">{{ alphaText }}</div>
        </div>
        <div class="form-group">
                  <label for="house">Дом</label>
                  <input @blur="$v.formReg.house.$touch()"
                     :class="status($v.formReg.house)"
                     v-model.trim="formReg.house"                  
                     type="house" class="form-control" id="house" placeholder="Например, 35">
                     <div class="invalid-feedback" v-if="!$v.formReg.house.numeric">{{ numText }}</div>                  
        </div>

              <button @click="step--" type="button" class="btn btn-light mr-2">Назад</button>
              <button @click="step++" :disabled="disabledBtn2"
                      type="button" class="btn btn-primary">Следующий шаг</button>

          </div>
        </transition>

        <transition name="slide-fade">
          <div v-show="step === 3">
              
              <div class="form-group">
                  <label for="document_type">Тип документа*</label>                  
                  <select @blur="$v.formReg.document_type.$touch()"
                          :class="status($v.formReg.document_type)"
                          v-model.trim="formReg.document_type"
                          id="document_type" class="form-control">
                      <option 
                       v-for="(document_type, index) in documents"
                       :value="document_type.value"
                       :key="index"
                      >
                        {{ document_type.label }}
                      </option>
                    </select>                  
                  <div class="invalid-feedback" v-if="!$v.formReg.document_type.required">{{ reqText }}</div>
              </div>
              <div class="form-group">
                <label for="seriesInputField">Серия и номер</label>
                  <input @blur="$v.formReg.seriesInputField.$touch()"
                     :class="status($v.formReg.seriesInputField)"
                     v-model.trim="formReg.seriesInputField"                  
                     type="seriesInputField" class="form-control" id="seriesInputField" placeholder="Например, 6104333111">
                  </div>
                  <div class="form-group">
                    <label for="issued">Кем выдан</label>
                    <input @blur="$v.formReg.issued.$touch()"
                     :class="status($v.formReg.issued)"
                     v-model.trim="formReg.issued"
                     type="issued" class="form-control" id="issued" placeholder="Например, Отделом внутренних дел одинцовского района города Москвы">
                    </div>
                    <div class="form-group">
                <label for="issue">Дата Выдачи*</label>
                <input @blur="$v.formReg.issue.$touch()"
                     :class="status($v.formReg.issue)"
                     v-model.trim="formReg.issue"                  
                     type="date" class="form-control" id="issue">
              <div class="invalid-feedback" v-if="$v.formReg.issue.$error">{{ reqText }}</div>
          </div>

          <div class="form-group">
              <label for="email">Email*</label>

              <input @blur="$v.formReg.email.$touch()"
                     :class="status($v.formReg.email)"
                     v-model.trim="formReg.email"
                     type="text" class="form-control" id="email">

              <div class="invalid-feedback" v-if="!$v.formReg.email.required">{{ reqText }}</div>
              <div class="invalid-feedback" v-if="!$v.formReg.email.email">Пожалуйста введите Email адрес</div>
          </div>

          <div class="form-group">
                  <label for="password">Пароль*</label>
                  
                  <input @blur="$v.formReg.password.$touch()"
                         :class="status($v.formReg.password)"
                         v-model.trim="formReg.password"
                         type="text" class="form-control" id="password">
                    
                  <div class="invalid-feedback" v-if="!$v.formReg.password.required">{{ reqText }}</div>
                  
              </div>

              <div class="form-group">
                  <label for="passwordConfirm">Подтверждение пароля*</label>
                  
                  <input @blur="$v.formReg.passwordConfirm.$touch()"
                         :class="status($v.formReg.passwordConfirm)"
                         v-model.trim="formReg.passwordConfirm"
                         type="text" class="form-control" id="passwordConfirm">
                  
                  <div class="invalid-feedback" 
                       v-if="!$v.formReg.passwordConfirm.sameAs">{{ passwordConfirmText }}</div>
              </div>

              <button @click="step--" type="button" class="btn btn-light mr-2">Назад</button>
              <button :disabled="disabledBtnFinish"
                      type="submit" class="btn btn-primary">Зарегистрироваться</button>

          </div>
        </transition>
        <small>*Поле обязательное для заполнения</small>
      </form>

    </div>
  </div>
</template>

<script>
import { required, helpers, numeric, minLength, maxLength, email, sameAs } from 'vuelidate/lib/validators'
const alpha = helpers.regex('alpha', /^[a-zA-Zа-яёА-ЯЁ]*$/, 'numeric', /^[0-9]*$/)
export default {
   data() {
     return {
        step: 1,
        regMessage: false,
        reqText: 'Поле обязательно для заполнения',
        alphaText: 'Запрещены цифры, пробелы и другие символы',
        numText: 'Формат записи числовой',
        minLengthText: 'Минимальная длина 11 символов!',
        maxLengthText: 'Максимальная длина 11 символов!',
        passwordConfirmText: 'Пароли не совпадают',    
        agreeWithSendSms: false, 
        attending_doctor: 'Ivanov',
        clients: [
                {
                    label: 'VIP',
                    value: 'VIP'
                },
                {
                    label: 'Проблемные',
                    value: 'Problematic'
                },
                {
                    label: 'ОМС',
                    value: 'OMS'
                },
            ],
            documents: [
                {
                    label: 'Паспорт',
                    value: 'Passport'
                },
                {
                    label: 'Свидетельство о рождении',
                    value: 'Birth certificate'
                },
                {
                    label: 'Вод. удостоверение',
                    value: 'Driving license'
                },
            ],        
        formReg: {
          last_name: '',
          first_name: '',
          birthdate: '',
          phone: '',
          favoriteClients: [],
          indInputField: '',
          country: '',
          region: '',
          city: '',
          street: '',
          house: '',
          document_type: '',
          seriesInputField: '',
          issued: '',
          issue: '',
          email: '',
          password: '',
          passwordConfirm: ''         
        },
            doctors: [
                {
                    label: 'Иванов',
                    value: 'Ivanov'
                },
                {
                    label: 'Захаров',
                    value: 'Zakharov'
                },
                {
                    label: 'Чернышева',
                    value: 'Chernysheva'
                },
            ],            
     }
   },
   computed: {
    disabledBtn1() {
       return this.$v.formReg.last_name.$invalid  ||
              this.$v.formReg.first_name.$invalid ||
              this.$v.formReg.birthdate.$invalid  ||
              this.$v.formReg.phone.$invalid      ||
              this.$v.formReg.favoriteClients.$invalid     
    },
    disabledBtn2() {
       return this.$v.formReg.city.$invalid
    },
    disabledBtnFinish() {
       return this.$v.formReg.document_type.$invalid ||
              this.$v.formReg.issue.$invalid ||
              this.$v.formReg.email.$invalid ||
              this.$v.formReg.password.$invalid || 
              this.$v.formReg.passwordConfirm.$invalid
    }
   },
   methods: {
    status(validation) {
       return {
         'is-invalid': validation.$error,
         'error': validation.$error
       }
    },
    userRegister() {
        console.group()
          console.log('Вы успешно зарегистрированны!')
          console.log('Фамилия: ' + this.formReg.last_name)
          console.log('Имя: ' + this.formReg.first_name)
          console.log('Отчество: ' + this.formReg.middle_name)  
          console.log('Дата рождения: ' + this.formReg.birthdate)
          console.log('Номер телефона: ' + this.formReg.phone)
          console.log('Группа клиентов: ' + this.formReg.favoriteClients)  
          console.log('Индекс: ' + this.formReg.indInputField)
          console.log('Страна: ' + this.formReg.country)
          console.log('Страна: ' + this.formReg.region)                              
          console.log('Город: ' + this.formReg.city)
          console.log('Улица: ' + this.formReg.street)          
          console.log('Дом: ' + this.formReg.house)           
          console.log('Тип документа: ' + this.formReg.document_type)
          console.log('Серия и номер: ' + this.formReg.seriesInputField)          
          console.log('Кем вылан: ' + this.formReg.issued)          
          console.log('Дата выдачи' + this.formReg.issue)
          console.log('Email: ' + this.formReg.email)
          console.log('Пароль: ' + this.formReg.password)
        console.groupEnd()
        this.reset()
        
    },
    reset() {
        // сбросить шаг и показать сообщение о регистрации
        this.step = 1;
        this.regMessage = true;
        // убрать сообщение о регистрации
        setTimeout(() => {
          this.regMessage = false
        }, 3000)
        // сбросить все поля
        for (let input in this.formReg) {
            this.formReg[input] = ''
        }
        // сбросить валидацию
        this.$v.$reset()
    }
  },
  validations: {
      formReg: {
          last_name: {
            required,
            alpha
          },
          first_name: {
            required,
            alpha
          },
          middle_name: {
            alpha
          },   
          birthdate: {
            required
          },
          phone: {
            required,
            numeric,
            minLength: minLength(11),
            maxLength: maxLength(11)
          },
          favoriteClients: {
            required
          },
          indInputField: {
            numeric
          },
          country: {
            alpha
          },
          region: {
          },          
          city: {
            required,
            alpha
          },
          street: {
            alpha
          },
          house: {
            numeric
          },      
          document_type: {
            required
          },
          seriesInputField: {
            numeric
          },
          issued: {
            alpha
          },
          issue: {
            required
          },
          email: {
            email,
            required
          },
          password: {
            required,
            minLength: minLength(6)
          },
          passwordConfirm: {
            sameAs: sameAs('password')
          }
      }
  },
}

</script>

<style lang="sass">
body 
    background-color: #f1f1f1

form 
    background-color: white
    padding: 20px
    border-radius: 10px
    box-shadow: 10px 10px 45px -31px rgba(0, 0, 0, 0.75)

.error 
    background-color: #fdd

.reg-title 
    color: #5d5d5d
    font-size: 24px
    margin-bottom: 18px
    padding-left: 20px
.form-check
    padding-left: 0px
.slide-fade-enter-active 
    transition: all 0.3s ease

.slide-fade-enter 
    transform: translateX(10px)
    opacity: 0
.btn
    margin: 10px 10px 0px 0px
</style>