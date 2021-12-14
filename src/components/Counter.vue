<template>
    <div v-if="loaded">
        <section class="text-3xl flex justify-center content-center flex-col mx-auto text-center">
            <h5 v-if="!expired">Buy now</h5>
            <h5 v-else>Timer is done</h5>
        </section>
        <section class="flex text-6xl justify-center content-center">
            <div class="days mr-2 relative">
                {{ displayDays }}
                <div class="label text-sm absolute bottom-0">
                    days
                </div>
            </div>
            <span class="leading-snug">:</span>
            <div class="hours mr-2 relative">
                {{ displayHours }}
                <div class="label text-sm absolute bottom-0">
                    hours
                </div>
            </div>
            <span class="leading-snug">:</span>
            <div class="munites mr-2 relative">
                {{ displayMunites }}
                <div class="label text-sm absolute bottom-0">
                    munites
                </div>
            </div>
            <span class="leading-snug">:</span>
            <div class="seconds mr-2 relative">
                {{ displaySeconds }}
                <div class="label text-sm absolute bottom-0">
                    seconds
                </div>
            </div>
        </section>
        
        
    </div>
</template>

<script>
    export default {
        props: ['year', 'month', 'date', 'hour', 'munite', 'second', 'millisecond'],
        data: () => ({
            displayDays: 0,
            displayHours: 0,
            displayMunites: 0,
            displaySeconds: 0,
            loaded: false,
            expired: false
        }),
        computed: {
            _seconds: () => 1000,
            _munites() {
                return this._seconds * 60;
            },
            _hours() {
                return this._munites * 60;
            },
            _days() {
                return this._hours * 24;
            },
            end() {
                return new Date(
                    this.year,
                    this.month,
                    this.date,
                    this.hour,
                    this.munite,
                    this.second,
                    this.millisecond
                )
            }
        },
        mounted() {
            this.showRemaining();
        },
        methods: {
            formatNum: num => (num < 10 ? '0' + num : num),
            showRemaining() {
                
                var timer = setInterval(() => {
                    var now = new Date();
                    // var end = new Date(2022, 4, 22, 10, 10, 10, 10);
                    var distance = this.end.getTime() - now.getTime();

                    if (distance < 0)
                    {
                        clearInterval(timer);
                        this.expired = true;
                        this.loaded = true;
                        return;
                    }

                    var days = Math.floor(distance / this._days);
                    var hours = Math.floor((distance % this._days) / this._hours);
                    var munites = Math.floor((distance % this._hours) / this._munites);
                    var seconds = Math.floor((distance % this._munites) / this._seconds);
                    this.displayMunites = this.formatNum(munites);
                    this.displaySeconds = this.formatNum(seconds);
                    this.displayHours = this.formatNum(hours);
                    this.displayDays = this.formatNum(days);
                    this.loaded = true;
                }, 1000);
            }
        }
    }
</script>

<style scoped>
    .seconds {
        max-width: 60px;
    }
</style>