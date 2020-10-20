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
				<ScCard class="uk-margin-top">
					<ScCardTitle>
						Cart
					</ScCardTitle>
					<ScCardBody>
						<div class="uk-overflow-auto">
							<table class="uk-table uk-table-hover uk-table-divider">
								<thead>
									<tr>
										<th class="uk-text-nowrap">
											id
										</th>
										<th class="uk-text-nowrap">
											Name
										</th>
										<th class="uk-text-nowrap">
											actions
										</th>
									</tr>
								</thead>
								<tbody>
									<tr v-for="record in records" :key="record.id">
										<td>{{ record.id }}</td>
										<td>{{ record.name }}</td>
										<td>
											<button class="sc-button sc-button-primary" type="button">
												Button
											</button>
										</td>
									</tr>
								</tbody>
							</table>
						</div>
					</ScCardBody>
				</ScCard>
			</div>
		</div>
	</div>
</template>
<script>
import Vue from 'vue'
import VueTypeahead from 'vue-typeahead'
import axios from 'axios'
Vue.prototype.$http = axios
var qs=require('qs')
export default {
	components:{
	},
	extends: VueTypeahead,
	data () {
		return {
			// src: 'https://typeahead-js-twitter-api-proxy.herokuapp.com/demo/search',
			src:'http://demo.com/json/web/index.php?r=details/view',
			limit: 5,
			minChars: 3,
			query:'',
			fitst_name:'',
			last_name:'',
			queryParamName: 'id',
			name:'',
			records:''
			
			
		}
	},
	methods: {
		onHit (item) {	  
			this.addRecord(item)
			this.getRecords()

		},
         
		add: function (item) {
			this.items.push(item);

		},

		addRecord (item){
		 axios.post('http://demo.com/json/web/index.php?r=cart/create', qs.stringify({
			
				name:item.first_name.concat(item.last_name)
              
			}), {	headers: {
				'Content-type': 'application/x-www-form-urlencoded',
			}
			})
            	.then(function (response) {
					// currentObj.output = response.data;
					console.log('posted')
				})
				
				.catch(function (error) {
					console.log(error);
				});

			
		},
		      getRecords (){
			axios.get('http://demo.com/json/web/index.php?r=cart/get').then((result)=>{
				console.log(result)
				this.records=result.data

          
			})
		},
	}}
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
  background-color: hsl(216, 44%, 33%);
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