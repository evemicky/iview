<template>
    <div v-if="showSizer || showElevator" :class="optsClasses">
        <div v-if="showSizer" :class="sizerClasses">
            <i-select :model.sync="pageSize" @on-change="changeSize">
                <i-option v-for="item in pageSizeOpts" :value="item" style="text-align:center;">{{ item }} 条/页</i-option>
            </i-select>
        </div>
        <div v-if="showElevator" :class="ElevatorClasses">
            跳至
            <input type="text" :value="_current" @keyup.enter="changePage">
            页
        </div>
    </div>
</template>
<script>
    import iSelect from '../../components/select/select.vue';
    import iOption from '../../components/select/option.vue';

    const prefixCls = 'ivu-page';

    function isValueNumber (value) {
        return (/^[1-9][0-9]*$/).test(value + '');
    }

    export default {
        components: { iSelect, iOption },
        props: {
            pageSizeOpts: Array,
            showSizer: Boolean,
            showElevator: Boolean,
            current: Number,
            _current: Number,
            pageSize: Number,
            allPages: Number
        },
        computed: {
            optsClasses () {
                return [
                    `${prefixCls}-options`
                ]
            },
            sizerClasses () {
                return [
                    `${prefixCls}-options-sizer`
                ]
            },
            ElevatorClasses () {
                return [
                    `${prefixCls}-options-elevator`
                ]
            }
        },
        methods: {
            changeSize () {
                this.$emit('on-size', this.pageSize);
            },
            changePage (event) {
                let val = event.target.value.trim();
                let page = 0;

                if (isValueNumber(val)) {
                    val = Number(val);
                    if (val != this.current) {
                        const allPages = this.allPages;

                        if (val > allPages) {
                            page = allPages;
                        } else {
                            page = val;
                        }
                    }
                } else {
                    page = 1;
                }

                if (page) {
                    this.$emit('on-page', page);
                    event.target.value = page;
                }
            }
        }
    }
</script>