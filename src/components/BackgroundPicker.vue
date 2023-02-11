<template>
	<div class="background-picker__main">
		<div class="btn-group dropright background-picker__picker">
			<button 
				class="btn btn-circle dropdown-toggle background-picker__picker-button" 
				data-toggle="dropdown" 
				aria-haspopup="true" 
				aria-expanded="false"
				id="dropdownPalette"
				:style="buttonStyle"
			>
				<svg v-show="!selectedItem" class="background-picker__picker-button-image" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg" transform="rotate(270)">
					<path fill-rule="evenodd" clip-rule="evenodd" 
					d="M12 1C5.92487 1 1 5.92487 1 12C1 18.0751 5.92487 23 12 23C18.0751 23 23 18.0751 23 12C23 5.92487 18.0751 1 12 1ZM6.38231 4.9681C7.92199 3.73647 9.87499 3 12 3C16.9706 3 21 7.02944 21 12C21 14.125 20.2635 16.078 19.0319 17.6177L6.38231 4.9681ZM4.9681 6.38231C3.73647 7.92199 3 9.87499 3 12C3 16.9706 7.02944 21 12 21C14.125 21 16.078 20.2635 17.6177 19.0319L4.9681 6.38231Z" 
					fill="#F0F0F0"></path> 
				</svg>
				<img 
					v-if="pickerType === 'image' && selectedItem" 
					class="background-picker__picker-button-image" 
					:src="selectedItem"
				/>
			</button>
			<div v-if="pickerType === 'color'" class="dropdown-menu background-picker__palette-colors">
				<div v-for="column in paletteItemsList" class="dropdown-col">
					<button 
						v-for="item in column"
						href="#" @click="setItem(item)"
						class="btn btn-circle dropdown-item background-picker__picker-button-palette" 
						:style="setColorPaletteStyle(item)"
					>
						<svg v-if="!item" class="background-picker__picker-button-image" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg" transform="rotate(270)">
							<path fill-rule="evenodd" clip-rule="evenodd" 
							d="M12 1C5.92487 1 1 5.92487 1 12C1 18.0751 5.92487 23 12 23C18.0751 23 23 18.0751 23 12C23 5.92487 18.0751 1 12 1ZM6.38231 4.9681C7.92199 3.73647 9.87499 3 12 3C16.9706 3 21 7.02944 21 12C21 14.125 20.2635 16.078 19.0319 17.6177L6.38231 4.9681ZM4.9681 6.38231C3.73647 7.92199 3 9.87499 3 12C3 16.9706 7.02944 21 12 21C14.125 21 16.078 20.2635 17.6177 19.0319L4.9681 6.38231Z" 
							fill="#F0F0F0"></path> 
						</svg>
					</button>
				</div>
			</div>
			<div v-if="pickerType === 'image'" class="dropdown-menu background-picker__palette-images">
				<div v-for="column in paletteItemsList" class="dropdown-col">
					<button 
						v-for="item in column"
						href="#" @click="setItem(item)"
						class="btn btn-circle dropdown-item background-picker__picker-button-palette" 
					>
						<svg v-if="!item" class="background-picker__picker-button-image" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg" transform="rotate(270)">
							<path fill-rule="evenodd" clip-rule="evenodd" 
							d="M12 1C5.92487 1 1 5.92487 1 12C1 18.0751 5.92487 23 12 23C18.0751 23 23 18.0751 23 12C23 5.92487 18.0751 1 12 1ZM6.38231 4.9681C7.92199 3.73647 9.87499 3 12 3C16.9706 3 21 7.02944 21 12C21 14.125 20.2635 16.078 19.0319 17.6177L6.38231 4.9681ZM4.9681 6.38231C3.73647 7.92199 3 9.87499 3 12C3 16.9706 7.02944 21 12 21C14.125 21 16.078 20.2635 17.6177 19.0319L4.9681 6.38231Z" 
							fill="#F0F0F0"></path> 
						</svg>
						<img
							v-else
							class="background-picker__picker-button-image" 
							:src="item"
						/>
					</button>
				</div>
			</div>
			<p class="background-picker__label">{{ label }}</p>
		</div>
	</div>
</template>

<script>
export default {
	name: "background-picker",
	props: {
		label: {
			type: String,
			default: ""
		},
		options: {
			type: Array
		},
		pickerType: {
			type: String,
			default: "color"
		},
		paletteColumns: {
			type: Number,
			default: 3
		},
		borderColor: {
			type: String,
			default: '#DE1B72'
		},
		size: {
			type: String,
			default: 'md'
		}, 
		clearSelection: {
			type: Boolean,
			default: false
		}
	},
	data() {
		return {
			selectedItem: "",
			itemsList: [],
			paletteItemsList: []
		}
	},
	computed: {
		buttonStyle() {
			let buttonInlineStyle="";
			if(this.selectedItem){
				if(this.pickerType === "color"){
					buttonInlineStyle += "background-color:" + this.selectedItem + ";"
				}
				buttonInlineStyle += "box-shadow: 0 0 0 0.1rem " + this.borderColor +"; border: 0.3rem solid white;"
			}
			return buttonInlineStyle;
		}
	},
	watch: {
		clearSelection: function(isClean) {
			isClean ? this.clear() : "";
		}
	},
	created() {
		//Add an empty item to be able to clear the selection inside the palette
		this.itemsList = [...this.options, ""];
		this.setPaletteItemsList();
	},
	methods: {
		setItem: function(item) {
			this.selectedItem = item;
			this.$emit('change', this.selectedItem, this.pickerType)
		},
		setColorPaletteStyle: function(item) {
			return ("background-color:" + item + ";")
		},
		setPaletteItemsList() {
			var j=0;
			while(j < this.itemsList.length){
				var columnItems = []
				for(let i=0; i < this.paletteColumns; i++){
					if(!this.paletteItemsList[i]) this.paletteItemsList[i] = [];
					if(j < this.itemsList.length) this.paletteItemsList[i].push(this.itemsList[j]);
					j++;
				}
			}
		},
		clear() {
			this.selectedItem = "";
		}
	}
}
</script>

<style scoped>
	p {
		color: inherit;
		font-size: inherit;
		font-weight: inherit;
	}

  	.background-picker__main {
		margin: 10px;
	}

	.background-picker__picker {
		display: block;
		text-align: center;
	}

	.background-picker__picker-button {
		overflow: hidden;
		border-radius: 30px;
		width: 50px;
    	height: 50px;
		background-color: #fff;
	}

	.background-picker__picker-button-image {
		position: absolute;
		width: 100%;
		top: 0;
		left: 0;
	}

	.background-picker__picker-button-palette {
		display: inline-block;
		overflow: hidden;
		width: 20px;
    	height: 20px;
		padding: 10px;
		margin: 10px;
		border-radius: 10px;
		box-shadow: 0 0 0 0.2rem rgb(240 240 240);
		margin-left: 0;
	}

	.background-picker__label {
		margin: 0;
	}

	.dropdown-toggle::after {
    	display: none;
	}

	.btn.focus, .btn:focus {
		outline: 0;
    	box-shadow: 0 0 0 0.2rem rgb(240 240 240);
	}

	.dropdown-menu.show {
		display: flex;
		flex-wrap: wrap;
		width: 3rem;
		padding: 5px 0 5px 10px ;
		text-align: center;
	}

	.dropdown-item {
		width: 33,33%;
		height: auto;
	}

	.dropdown-col {
		display:inline-block;
		width: 33%;
	}
</style>