<template>
    <div v-if="! celebrating" class="background" v-bind:style="backgroundStyleObject"></div>
    <canvas id="countdown-canvas"></canvas>
    <h1 v-if="! celebrating" class="location">
        <p class="location__name">{{ location }}</p>
    </h1>

    <h1 v-if="celebrating" class="happy-new-year">Happy New Year!</h1>

    <div class="countdown">
        <div class="block">
            <p class="digit">{{ minutesRemaining }}:{{ secondsRemaining }}</p>
        </div>
    </div><!-- .countdown -->
</template>
<script>

export default {

    ready() {
        this.celebrating = false;
        window.setInterval(() => {
            this.now = new Date();
            if (
                ! this.celebrating
                && 0 == this.minutesRemaining
                && 0 == this.secondsRemaining
            ) {
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
            now: new Date(),
            celebrating: false,
            celebrationDuration: 15, // in seconds
            locations: {
                '0': {
                    'name': 'Praia, Cape Verde',
                    'image': 'praia-cape-verde.jpg',
                    'imageSource': 'https://commons.wikimedia.org/wiki/File:Praia_aerial.jpg'
                },
                '1': {
                    'name': 'Rio de Janeiro, Brazil',
                    'image': 'rio-de-janeiro-brazil.jpg',
                    'imageSource': 'https://pixabay.com/en/rio-de-janeiro-brazil-city-urban-1963744/'
                },
                '2': {
                    'name': 'Buenos Aires, Argentina',
                    'image': 'buenos-aires-argentia.jpg',
                    'imageSource': 'https://www.flickr.com/photos/gameoflight/10363371446'
                },
                '3': {
                    'name': 'Halifax, Nova Scotia, Canada',
                    'image': 'halifax-nova-scotia.jpg',
                    'imageSource': 'https://pixabay.com/en/halifax-nova-scotia-canada-city-2380861/'
                },
                '4': {
                    'name': 'Reading, PA',
                    'image': 'reading-pa.jpg'
                },
                '20': {
                    'name': 'Moscow, Russia',
                    'image': 'moscow-russia.jpg',
                    'imageSource': 'http://maxpixel.freegreatpicture.com/Capital-Moscow-Russia-Kremlin-Historically-538791'
                },
                '21': {
                    'name': 'Cairo, Egypt',
                    'image': 'cairo-egypt.jpg',
                    'imageSource': 'https://commons.wikimedia.org/wiki/File:Pyramid_and_sphinx_of_cairo,_egypt.jpg'
                },
                '22': {
                    'name': 'Berlin, Germany',
                    'image': 'berlin-germany.jpg',
                    'imageSource': 'https://pixabay.com/en/berlin-germany-berlin-cathedral-2975784/'
                },
                '23': {
                    'name': 'London, England',
                    'image': 'london-england.jpg',
                    'imageSource': 'http://maxpixel.freegreatpicture.com/London-England-Clock-Big-Ben-Parliament-2626787'
                }
            }
        }
    },

    computed: {
        secondsRemaining() {
            return this.addLeadingZero( 59 - this.now.getUTCSeconds() );
        },

        minutesRemaining() {
            return this.addLeadingZero( 59 - this.now.getUTCMinutes() );
        },

        hours() {
            return this.now.getUTCHours();
        },

		location() {
			return this.getLocation().name;
		},

        backgroundStyleObject() {
            return {
                'backgroundImage':
                'url(images/' + this.getLocation().image + ')'
            };
        },
    },
	methods: {
        addLeadingZero( number ) {
            if ( number > 9 ) {
                return number;
            }
            return '0' + number;
        },
        getLocation() {
            return this.locations[ this.hours ] ? this.locations[ this.hours ] : {};
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
            }, this.celebrationDuration * 1000 );
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
    z-index: 3;
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
    z-index: 2;
}
.background {
    background-size: cover;
    bottom: 0;
    left: 0;
    opacity: 0.5;
    position: absolute;
    right: 0;
    top: 0;
    width: 100%;
    z-index: 1;
}
.happy-new-year {
    color: #fff;
    font-size: 100px;
    margin: 0;
    position: absolute;
    text-align: center;
    top: 50%;
    transform: translateY(-50%);
    width: 100%;
}
</style>
