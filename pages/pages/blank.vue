<template>
	<div id="sc-page-wrapper">
		<div id="sc-page-content">
			<div class="Typeahead">
				<i v-if="loading" class="fa fa-spinner fa-spin"></i>
				<template v-else>
					<i v-show="isEmpty" class="fa fa-search"></i>
					<i v-show="isDirty" class="fa fa-times" @click="reset"></i>
				</template>

				<input
					v-model="query"
					type="text"
					class="Typeahead__input"
					placeholder="Search user"
					autocomplete="off"
					@keydown.down="down"
					@keydown.up="up"
					@keydown.enter="hit"
					@keydown.esc="reset"
					@blur="reset"
					@input="update"
				>

				<ul v-show="hasItems">
					<li v-for="(item, $item) in items"
						:key="item.id"
						:class="activeClass($item)"
						@mousedown="hit"
						@mousemove="setActive($item)"
					>
						<span class="name" v-text="item.first_name"></span>
						<span class="screen-name" v-text="item.last_name"></span>
					</li>
				</ul>
			</div>
		</div>
		<div class="uk-flex-center uk-grid" data-uk-grid>
			<div class="uk-width-3-5@l">
				<ScCard>
					<ScCardBody>
						<form>
							<fieldset class="uk-fieldset md-bg-grey-100 sc-padding">
								<p class="sc-text-semibold uk-text-large uk-margin-remove-top">
									Personal info
								</p>
								<div class="uk-child-width-1-2@m uk-grid" data-uk-grid>
									<div>
										<label class="uk-form-label" for="f-f-name">
											First Name
										</label>
										<div class="uk-form-controls">
											<ScInput id="f-f-name" v-model="fName" mode="outline"></ScInput>
										</div>
									</div>
									<div>
										<label class="uk-form-label" for="f-l-name">
											Last Name
										</label>
										<div class="uk-form-controls">
											<ScInput id="f-l-name" v-model="lName" mode="outline"></ScInput>
										</div>
									</div>
								</div>
							</fieldset>
							<div class="uk-margin-top">
								<button class="sc-button sc-button-primary" type="button">
									Update
								</button>
							</div>
						</form>
						<!-- <pre class="uk-margin-top">{{ userData | json }}</pre> -->
					</ScCardBody>
				</ScCard>
			</div>
		</div>
	</div>
</template>
<script>
import Vue from 'vue'
import VueTypeahead from 'vue-typeahead'
import ScInput from '~/components/Input'
import axios from 'axios'
Vue.prototype.$http = axios
export default {
	components:{
		ScInput
	},
	extends: VueTypeahead,
	data () {
		return {
			// src: 'https://typeahead-js-twitter-api-proxy.herokuapp.com/demo/search',
			src:'http://demo.com/json/web/index.php?r=details/view',
			limit: 5,
			minChars: 3,
			query:'',
			fName:'',
			lName:'',
			queryParamName: 'id'
			
		}
	},
	methods: {
		onHit (item) {
			// window.location.href = 'http://twitter.com/' + item.screen_name
			this.fName=item.first_name
			this.lName=item.last_name
		}
	}
}
</script>



<style scoped>
.Typeahead {
  position: relative;
}
.Typeahead__input {
  width: 100%;
  font-size: 14px;
  color: #2c3e50;
  line-height: 1.42857143;
  box-shadow: inset 0 1px 4px rgba(0,0,0,.4);
  -webkit-transition: border-color ease-in-out .15s,-webkit-box-shadow ease-in-out .15s;
  transition: border-color ease-in-out .15s,box-shadow ease-in-out .15s;
  font-weight: 300;
  padding: 12px 26px;
  border: none;
  border-radius: 22px;
  letter-spacing: 1px;
  box-sizing: border-box;
}
.Typeahead__input:focus {
  border-color: #4fc08d;
  outline: 0;
  box-shadow: inset 0 1px 1px rgba(0,0,0,.075),0 0 8px #4fc08d;
}
.fa-times {
  cursor: pointer;
}
i {
  float: right;
  position: relative;
  top: 30px;
  right: 29px;
  opacity: 0.4;
}
ul {
  position: absolute;
  padding: 0;
  margin-top: 8px;
  min-width: 100%;
  background-color: #fff;
  list-style: none;
  border-radius: 4px;
  box-shadow: 0 0 10px rgba(0,0,0, 0.25);
  z-index: 1000;
}
li {
  padding: 10px 16px;
  border-bottom: 1px solid #ccc;
  cursor: pointer;
}
li:first-child {
  border-top-left-radius: 4px;
  border-top-right-radius: 4px;
}
li:last-child {
  border-bottom-left-radius: 4px;
  border-bottom-right-radius: 4px;
  border-bottom: 0;
}
span {
  display: block;
  color: #2c3e50;
}
.active {
  background-color: #3aa373;
}
.active span {
  color: white;
}
.name {
  font-weight: 700;
  font-size: 18px;
}
.screen-name {
  font-style: italic;
}
</style>