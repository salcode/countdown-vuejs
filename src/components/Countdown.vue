<template>
    <canvas id="countdown-canvas"></canvas>
    <h1 class="location">
        <p class="location__name">{{ location }}</p>
    </h1>

    <div class="countdown">
        <div class="block">
            <p class="digit">{{ minutes }}:{{ seconds }}</p>
        </div>
    </div><!-- .countdown -->
</template>
<script>

export default {

    ready() {
        this.celebrating = false;
        window.setInterval(() => {
            this.now = Math.trunc((new Date()).getTime() / 1000);
            this.secondsRemaining = this.getSecondsRemaining();
            if ( ! this.celebrating && 0 === this.secondsRemaining % 60 ) {
                this.celebrate();
            }
        },250);
    },

    props : {
        date : {
            type: Number,
            coerce: str => Math.trunc(Date.parse(str) / 1000)
        }
    },

    data() {
        return {
            now: Math.trunc((new Date()).getTime() / 1000),
            celebrating: false,
            secondsRemaining: this.getSecondsRemaining()
        }
    },

    computed: {
        seconds() {
            return this.addLeadingZero( this.secondsRemaining % 60 );
        },

        minutes() {
            return Math.trunc( this.secondsRemaining / 60) % 60;
        },

        hours() {
            return Math.trunc( this.secondsRemaining / 60 / 60) % 24;
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
            return locations[ this.hours ] ? locations[ this.hours ] : 'Location ' + this.hours;
        },
        getSecondsRemaining() {
            return this.date - Math.trunc(new Date().getTime() / 1000 );
        },
        celebrate() {
            this.celebrating = true;
            this.hideCountdown();
            confettiStart();
            var that = this;
            setTimeout( function() {
                that.showCountdown();
                confettiStop();
                that.celebrating = false;
            }, 5000 );
        },
        hideCountdown() {
            document.getElementsByClassName('countdown')[0].classList.add('hide-me');
        },
        showCountdown() {
            document.getElementsByClassName('countdown')[0].classList.remove('hide-me');
        },
	}
}

</script>
<style>
@import url(https://fonts.googleapis.com/css?family=Roboto+Condensed:400|Roboto:100);

.location {
    color: #fff;
    font-weight: normal;
    position: relative;
    text-align: center;
    z-index: 2;
}
.countdown.hide-me {
    display: none;
}
.countdown {
    align-items: center;
    bottom: 0;
    display: flex;
    left: 0;
    position: absolute;
    justify-content: center;
    right: 0;
    top: 0;
    z-index: 2;
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
#countdown-canvas {
    bottom: 0;
    left: 0;
    position: absolute;
    right: 0;
    top: 0;
    width: 100%;
    z-index: 1;
}
</style>
