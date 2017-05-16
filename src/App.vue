<template>
	<v-app>
		<v-row>
			<v-col xs12 sm10 offset-sm1>
				<v-card class="mt-4">
					<v-card-row class="grey darken-4">
						<v-card-title>
							<span class="white--text">CIFRA DE CESAR</span>
						</v-card-title>
					</v-card-row>
					<v-card-row>
						<v-card-text class="grey lighten-4">
							<v-text-field v-model="decriptado" @input="encode" multi-line label="Texto decriptado"></v-text-field>
							<v-text-field v-model="encriptado" @input="decode" multi-line label="Texto encriptado"></v-text-field>
						</v-card-text>
					</v-card-row>
					<v-card-row actions class="grey darken-2">
						<v-menu transition="v-slide-y-transition" max-height="250">
							<v-btn slot="activator" class="accent white--text">
								Chave: {{chave}}
							</v-btn>
							<v-list>
								<v-list-item v-for="n in 26" :key="n" @click="setarChave(n)">
									<v-list-tile>
										<v-list-tile-title>{{n}}</v-list-tile-title>
									</v-list-tile>
								</v-list-item>
							</v-list>
						</v-menu>
						<v-spacer></v-spacer>
					</v-card-row>
				</v-card>
			</v-col>
		</v-row>
	</v-app>
</template>

<script>
export default {
	data(){
		return {
			chave: 1,
			encriptado: '',
			decriptado: ''
		}
	},
	methods: {
		encode(){
			this.encriptado = '';
			for(let i = 0; i < this.decriptado.length; i++){
				if(/[a-z]/i.test(this.decriptado.charAt(i))){
					let char = String.fromCharCode(((this.decriptado.toLowerCase().charCodeAt(i) + this.chave) - 97) % 26 + 97);
					if(/[A-Z]/.test(this.decriptado.charAt(i))){
						char = char.toUpperCase();
					}
					this.encriptado += char;
				}
				else {
					this.encriptado += this.decriptado.charAt(i);
				}
			}
		},
		decode(){
			this.decriptado = '';
			for(let i = 0; i < this.encriptado.length; i++){
				if(/[a-z]/i.test(this.encriptado.charAt(i))){
					let transform = (this.encriptado.toLowerCase().charCodeAt(i) - this.chave) - 97;
					let char = String.fromCharCode((transform < 0 ? 26 + transform : transform) % 26 + 97);
					if(/[A-Z]/.test(this.encriptado.charAt(i))){
						char = char.toUpperCase();
					}
					this.decriptado += char;
				}
				else {
					this.decriptado += this.encriptado.charAt(i);
				}
			}
		},
		setarChave(chave){
			this.chave = chave;
			this.encode();
		}
	}
}
</script>

<style lang="stylus">
@import '../node_modules/vuetify/src/stylus/main'
</style>
