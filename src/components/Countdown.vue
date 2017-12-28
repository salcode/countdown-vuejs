<template>
    <div class="location">
        <p class="location__name">{{ location }}</p>
    </div>

    <div class="countdown">
        <div class="block">
            <p class="digit">{{ minutes }} : {{ seconds }}</p>
        </div>
    </div><!-- .countdown -->
</template>
<script>
export default {

    ready() {
        window.setInterval(() => {
            this.now = Math.trunc((new Date()).getTime() / 1000);
        },1000);
    },

    props : {
        date : {
            type: Number,
            coerce: str => Math.trunc(Date.parse(str) / 1000)
        }
    },

    data() {
        return {
            now: Math.trunc((new Date()).getTime() / 1000)
        }
    },

    computed: {
        seconds() {
            return this.addLeadingZero( (this.date - this.now) % 60 );
        },

        minutes() {
            return Math.trunc((this.date - this.now) / 60) % 60;
        },

        hours() {
            return Math.trunc((this.date - this.now) / 60 / 60) % 24;
        },

		location() {
			return this.getLocation();
		}
    },
	methods: {
        addLeadingZero( number ) {
            if ( number > 9 ) {
                return number;
            }
            return '0' + number;
        },
		getLocation() {
			var locations = {
				'0': 'Reading, PA',
				'1': 'Halifax, Nova Scotia, Canada',
				'2': 'Buenos Aires, Argentina',
				'3': 'Rio de Janeiro, Brazil',
				'4': 'Praia, Cape Verde',
				'5': 'London, England',
				'6': 'Berlin, Germany',
			};
			return locations[ this.hours ];
		}
	}
}

</script>
<style>
@import url(https://fonts.googleapis.com/css?family=Roboto+Condensed:400|Roboto:100);

.countdown {
    align-items: center;
    display: flex;
    justify-content: center;
}

.block {
    display: flex;
    flex-direction: column;
    margin: 20px;
}

.digit {
    color: #ecf0f1;
    font-size: 150px;
    font-weight: 100;
    font-family: 'Roboto', serif;
    margin: 10px;
    text-align: center;
}
</style>
