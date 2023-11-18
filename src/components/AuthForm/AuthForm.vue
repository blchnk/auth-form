<template>
    <div class="container">
        <form class="form" @submit.prevent="submitHandler()">
            <section class="section">
                <h3 class="title">Укажите свой ФИО</h3>

                <div class="section__section-wrapper">
                    <div class="input-wrapper">
                        <input class="input" :class="{ 'invalid': $v.lastName.$dirty && !$v.lastName.required }" type="text"
                            placeholder="Фамилия*" v-model.trim="$v.lastName.$model" />
                        <span v-if='$v.lastName.$dirty && $v.lastName.$error' class="error-message">Введите
                            фамилию</span>
                    </div>
                    <div class="input-wrapper">
                        <input class="input" :class="{ 'invalid': $v.firstName.$dirty && !$v.firstName.required }"
                            type="text" placeholder="Имя*" v-model.trim="$v.firstName.$model" />
                        <span v-if='$v.firstName.$dirty && $v.firstName.$error' class="error-message">Введите
                            имя</span>
                    </div>
                    <div class="input-wrapper">
                        <input class='input' type="text" placeholder="Отчество">
                    </div>
                </div>

                <div class="section__section-wrapper">
                    <label class="label" for="birth-date">
                        Дата рождения*
                        <div class="input-wraper">
                            <input class="input" name="birth-date" type="date"
                                :class="{ 'invalid': $v.birthDate.$dirty && !$v.birthDate.required }"
                                v-model.trim="$v.birthDate.$model" placeholder="дд.мм.гг" />
                            <span class="error-message" v-if="$v.birthDate.$dirty && !$v.birthDate.required">Укажите дату
                                рождения</span>
                        </div>
                    </label>
                    <label class="label" for="tel">
                        Номер телефона*
                        <div class="input-wrapper">
                            <input class="input" name="tel" type="text" :class="{ 'invalid': $v.phoneNumber.$error }"
                                v-model.trim="$v.phoneNumber.$model" placeholder="7-xxx-xxx-xxx-x" />
                            <span class="error-message" v-if="$v.phoneNumber.$error && !$v.phoneNumber.required">
                                Введите номер телефона
                            </span>
                            <span class="error-message" v-else-if="$v.phoneNumber.$error && !$v.phoneNumber.telPatern">
                                Номер не может состоять из букв
                            </span>
                            <span class="error-message"
                                v-else-if="$v.phoneNumber.$error && !$v.phoneNumber.checkFirstLetter">
                                Номер должен начинаться с 7
                            </span>
                            <span class="error-message"
                                v-else-if="$v.phoneNumber.$error && (!$v.phoneNumber.minLength || !$v.phoneNumber.telPatern)">
                                Необходимо {{ $v.phoneNumber.$params.minLength.min }} цифр
                            </span>
                            <span class="error-message"
                                v-else-if="$v.phoneNumber.$error && (!$v.phoneNumber.maxLength || !$v.phoneNumber.telPatern)">
                                Некорректный формат телефона
                            </span>
                        </div>
                    </label>
                    <h3 class="label">
                        Укажите ваш пол
                        <div class="input-wraper">
                            <div class="radio-group">
                                <label class="label label__gender" for="male">
                                    Мужчина
                                    <input type="radio" name="gender" id="male">
                                </label>
                                <label class="label label__gender" for="female">
                                    Женщина
                                    <input type="radio" name="gender" id="female">
                                </label>
                            </div>
                        </div>
                    </h3>
                </div>
                <div class="section__section-wrapper">
                    <label class="label" for="client-group">
                        Группа клиентов*
                        <select class="select" multiple name="client-group" v-model="$v.clientGroup.$model">
                            <option v-for="option in options.clientGroupOption" :value="option" :key="option">
                                {{ option }}
                            </option>
                        </select>
                    </label>
                    <label class="label" for="doctor">
                        Лечащий врач
                        <select class="select" name="doctor">
                            <option value="" selected disabled>Выберите значение</option>
                            <option v-for="option in options.doctorOption" :value="option" :key="option">
                                {{ option }}
                            </option>
                        </select>
                    </label>
                    <label class="label flex gap-5" for="sms">
                        Не отправлять СМС
                        <input class="input checkbox sms-checkbox mb-4" name="sms" type="checkbox" id="sms">
                    </label>
                </div>
            </section>
            <section class="section">
                <h3 class="title">Адрес:</h3>
                <div class="section__section-wrapper address-section-wrapper">
                    <input class="input input__address" type="text" placeholder="Индекс">
                    <input class="input input__address" type="text" placeholder="Страна">
                    <input class="input input__address" type="text" placeholder="Область">

                    <input class="input input__address" :class="{ 'invalid': $v.city.$dirty && !$v.city.required }"
                        v-model.trim="$v.city.$model" type="text" placeholder="Город*">
                    <input class="input input__address" type="text" placeholder="Улица">
                    <input class="input input__address" type="text" placeholder="Дом">
                </div>
            </section>
            <section class="section">
                <div class="section__section-wrapper">
                    <label class="label" for="document-type">
                        Тип документа*
                        <select class="select" name="document-type" v-model="$v.documentGroup.$model">
                            <option value="" selected disabled>Выберите значение</option>
                            <option v-for="option in options.documentOption" :value="option" :key="option">
                                {{ option }}
                            </option>
                        </select>
                    </label>
                    <label class="label" for="dateOfIssue">
                        Дата выдачи*
                        <input v-model="$v.dateOfIssue.$model" class="input"
                            :class="{ 'invalid': $v.dateOfIssue.$dirty && !$v.dateOfIssue.required }" name="dateOfIssue"
                            type="date" placeholder="дд.мм.гг">
                    </label>
                </div>
                <div class="section__section-wrapper">
                    <input class="input" type="text" placeholder="Серия">
                    <input class="input" type="text" placeholder="Номер">
                    <input class="input" type="text" placeholder="Кем выдан">
                </div>
            </section>
            <div class="btn-wrapper">
                <button class="btn" :class="$v.$invalid ? 'disabledBtn' : 'submit-btn'" :disabled="$v.$invalid">
                    Отправить
                </button>
            </div>
        </form>
    </div>
</template>
  
<script>
import { required, minLength, maxLength } from 'vuelidate/lib/validators'

const letterPatern = (/^[а-яА-ЯёЁa-zA-Z]+$/);

export default {
    name: 'AuthForm',
    components: {
    },
    data() {
        return {
            lastName: '',
            firstName: '',
            birthDate: '',
            phoneNumber: '',
            clientGroup: [],
            isSubmitted: false,
            city: '',
            dateOfIssue: '',
            documentGroup: [],
        }
    },
    validations: {
        lastName: {
            required,
            patern: (value) => letterPatern.test(value)
        },
        firstName: {
            required,
            patern: (value) => letterPatern.test(value)
        },
        birthDate: {
            required
        },
        phoneNumber: {
            required,
            minLength: minLength(11),
            maxLength: maxLength(11),
            checkFirstLetter: (value) => value[0] === '7',
            telPatern: (value) => (/^[0-9]+$/).test(value),
        },
        clientGroup: {
            required
        },
        city: {
            required,
            cityPatern: (value) => letterPatern.test(value),
        },
        dateOfIssue: {
            required
        },
        documentGroup: {
            required
        }
    },
    methods: {
        submitHandler() {
            if (!this.$v.$invalid) {
                alert('Новый клиент создан успешно!');
            }
        }
    },
    setup() {
        const options = {
            clientGroupOption: {
                1: 'VIP',
                2: 'Проблемные',
                3: 'ОМС'
            },
            doctorOption: {
                1: 'Иванов',
                2: 'Захаров',
                3: 'Чернышёва'
            },
            documentOption: {
                1: 'Паспорт',
                2: 'Свидетельство о рождении',
                3: 'Вод. удостоверение'
            }
        }

        return {
            options,
        }
    }
}
</script>
  
<style lang="scss"></style>
  