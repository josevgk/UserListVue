<template>
    <v-container>


        <v-text-field label="Buscar" v-model="search"/>
        <v-data-table :search="search" :items="users" :headers="headers" @click:row="_clickRow">
            <template v-slot:item.action="{ item }">
                <v-btn @click.prevent.stop="_showDetail(item)" icon>
                    <v-icon color="primary">mdi-account-card-details-outline</v-icon>
                </v-btn>
                <v-btn @click.prevent.stop="_edit(item)" icon>
                    <v-icon>mdi-file-edit-outline</v-icon>
                </v-btn>
            </template>
            <template v-slot:item.birth="{ item }">
                {{ _convertDate(item.birth) }}
            </template>

        </v-data-table>
        <v-row align="center">
            <v-col align-self="center" class="text-center">
                <v-btn @click="_addUser" class="primary">
                    {{ 'Nuevo Usuario' }}
                </v-btn>
            </v-col>

        </v-row>


        <Detalle :items="detalleItems" v-model="detalleActivo"/>
        <DialogUser @save="_saveUser" :model="dialogModel" v-model="dialogActivo"/>


    </v-container>
</template>

<script>
	import Users from '../assets/users';
	import Detalle from './Detalle';
	import DialogUser from './DialogUser';
	import lodash from 'lodash';

	export default {
		name: 'HelloWorld',
		components: {
			Detalle,
			DialogUser
		},

		data: () => (
			{

				users: Users,
				search: '',
				headers: [
					{
						text: 'Nombre',
						value: 'name'
					},
					{
						text: 'Apellidos',
						value: 'surname'
					},
					{
						text: 'Fecha de Nacimiento',
						value: 'birth'
					},
					{
						text: '',
						value: 'action',
						align: 'right',
						sortable: false
					}],

				detalleItems: [],
				detalleActivo: false,
				dialogActivo: false,
				dialogModel: {}
			}
		),

		methods: {
			_clickRow (item) {
				// eslint-disable-next-line no-console
				this.detalleItems = [item];
				this.detalleActivo = true;
			},

			_showDetail (item) {
				// eslint-disable-next-line no-console
				this.detalleItems = [item];
				this.detalleActivo = true;
			},

			_edit (item) {
				this.dialogModel = lodash.cloneDeep(item);
				this.dialogActivo = true;
			},

			_convertDate (date) {
				return window.moment(date).format('DD/MM/YYYY');
			},

			_addUser () {
				this.dialogModel = {};
				this.dialogActivo = true;
			},

			_saveUser (user) {
				let added = false;

				this.users.forEach((x, index) => {
					if ( x.id === user.id ) {
						added = true;
						this.users.splice(index, 1, user);
					}
				});

				if ( !added ) {
					this.users.push(user);
				}

			}
		}
	};
</script>
