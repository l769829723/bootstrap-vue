<template>
    <button :class="classObject"
            type="button"
            :aria-label="label"
            :aria-controls="target.id ? target.id : target"
            :aria-expanded="toggleState ? 'true' : 'false'"
            @click="onclick"
    >
        <slot><span class="navbar-toggler-icon"></span></slot>
    </button>
</template>

<script>
import { listenOnRootMixin } from '../../mixins';

export default {
    mixins: [listenOnRootMixin],
    computed: {
        classObject() {
            return [
                'navbar-toggler',
                'navbar-toggler-' + this.position
            ];
        }
    },
    data() {
        return {
            toggleState: false
        };
    },
    props: {
        label: {
            type: String,
            default: 'Toggle navigation'
        },
        position: {
            type: String,
            default: 'right'
        },
        target: {
            type: String,
            required: true
        }
    },
    methods: {
        onclick() {
            this.$root.$emit('bv::toggle::collapse', this.target);
        },
        handleStateEvt(id, state) {
            if (id === this.target) {
                this.toggleState = state;
            }
        }
    },
    created() {
        this.listenOnRoot('bv::collapse::state', this.handleStateEvt);
    }
};
</script>
