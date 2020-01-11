<template>
    <div class="container">
        <div class="row mt-5">
            <div class="col-md-12">
                <div class="card">
                    <div class="card-header">
                        <h3 class="card-title">Таблица пользователей</h3>

                        <div class="card-tools">
                            <button class="btn btn-success" data-toggle="modal" data-target="#addNew">Добавить нового <i class="fas fa-user-plus"></i></button>
                        </div>
                    </div>
                    <!-- /.card-header -->
                    <div class="card-body table-responsive no-padding">
                        <table class="table table-hover">
                            <tbody><tr>
                                <th>ИД</th>
                                <th>Имя</th>
                                <th>Email</th>
                                <th>Тип</th>
                                <th>Описание</th>
                                <th>Дата регистрации</th>
                                <th>Дата последнего обновления</th>
                                <th>Изменить</th>
                            </tr>
                            <tr v-for="user in users" :key="user.id">

                                <td v-if="user.id" class="indigo">{{ user.id }}</td>
                                <td v-else class="red">404</td>

                                <td v-if="user.name" class="indigo">{{ user.name | upText}}</td>
                                <td v-else class="red">404</td>

                                <td v-if="user.email" class="indigo">{{ user.email }}</td>
                                <td v-else class="red">404</td>

                                <td v-if="user.type" class="indigo">{{ user.type | upText}}</td>
                                <td v-else class="red">404</td>

                                <td v-if="user.bio" class="indigo">{{ user.bio }}</td>
                                <td v-else class="red">404</td>

                                <td v-if="user.created_at" class="indigo">{{ user.created_at | myDate}}</td>
                                <td v-else class="red">404</td>

                                <td v-if="user.updated_at" class="indigo">{{ user.updated_at | myDate}}</td>
                                <td v-else class="red">404</td>
                                <td>
                                    <a href="#">
                                        <i class="fa fa-edit blue"></i>
                                    </a>
                                    <a href="#">
                                        <i class="fa fa-trash red"></i>
                                    </a>
                                </td>
                            </tr>
                            </tbody></table>
                    </div>
                    <!-- /.card-body -->
                </div>
                <!-- /.card -->
            </div>
        </div>
        <!-- Modal -->
        <div class="modal fade" id="addNew" tabindex="-1" role="dialog" aria-labelledby="addNewLabel" aria-hidden="true">
            <div class="modal-dialog" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="addNewLabel">Добавление нового пользователя</h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <form @submit.prevent="createUser">
                        <div class="modal-body">
                            <div class="form-group">
                                <input
                                    placeholder="Имя"
                                    v-model="form.name"
                                    type="text"
                                    name="name"
                                    class="form-control"
                                    :class="{'is-invalid': form.errors.has('name')}"
                                >
                                <has-error :form="form" field="name"></has-error>
                            </div>
                            <div class="form-group">
                                <input
                                    placeholder="Email"
                                    v-model="form.email"
                                    type="text"
                                    name="email"
                                    class="form-control"
                                    :class="{'is-invalid': form.errors.has('email')}"
                                >
                                <has-error :form="form" field="email"></has-error>
                            </div>
                            <div class="form-group">
                                <textarea
                                    name="bio"
                                    id="bio"
                                    v-model="form.bio"
                                    placeholder="Краткая биография пользователя. Необязательно!"
                                    class="form-control"
                                    :class="{ 'is-invalid': form.errors.has('bio') }"
                                ></textarea>
                                <has-error :form="form" field="email"></has-error>
                            </div>
                            <div class="form-group">
                                <select name="type" v-model="form.type" id="type" class="form-control" :class="{
                                'is-invalid': form.errors.has('type') }">
                                    <option value="">Выберите роль пользователя</option>
                                    <option value="admin">Админ</option>
                                    <option value="user">Обычный пользователь</option>
                                    <option value="author">Автор</option>
                                </select>
                                <has-error :form="form" field="type"></has-error>
                            </div>
                            <div class="form-group">
                                <input
                                    placeholder="Пароль"
                                    v-model="form.password"
                                    type="password"
                                    name="password"
                                    class="form-control"
                                    :class="{'is-invalid': form.errors.has('password')}"
                                >
                                <has-error :form="form" field="password"></has-error>
                            </div>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-dismiss="modal">Отмена</button>
                            <button type="submit" class="btn btn-primary">Сохранить изменения</button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                users : {},
                form: new Form ({
                    name: '',
                    email: '',
                    password: '',
                    type: '',
                    bio: '',
                    photo: ''
                })
            }
        },
        methods: {
            loadUsers() {
                axios.get("api/user").then(({ data }) => (this.users = data.data));
            },
            createUser() {
                this.$Progress.start();
                this.form.post('api/user');
                $('#addNew').modal('hide');
                Toast.fire({
                    icon: 'success',
                    title: 'Новый пользоваетль успешно добавлен!'
                })
                this.$Progress.finish();
            }
        },
        mounted() {
            this.loadUsers();
            setInterval(() => this.loadUsers(), 3000);
        }
    }
</script>
