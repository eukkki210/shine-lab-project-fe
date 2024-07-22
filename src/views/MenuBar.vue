<script setup>
const emits = defineEmits(['displayData', 'clearData']);

// 각 버튼의 온클릭 이벤트
const onCompanyClick = () => {
    // console.log('조회 버튼 클릭');
    emits('displayCompanyData');
};

const onUserClick = () => {
    // console.log('조회 버튼 클릭');
    emits('displayUserData');
};

const onClearClick = () => {
    // console.log('초기화 버튼 클릭');
    emits('clearData');
};
</script>

<template>
    <div class="grid">
        <div class="col-12">
            <div class="card">
                <h5>Menu Bar</h5>
                <Toolbar>
                    <template v-slot:start>
                        <Button label="Display Companies" class="mr-2" @click="onCompanyClick" />
                        <Button label="Display Users" class="mr-2" @click="onUserClick" />
                        <Button label="Clear" icon="pi pi-filter-slash" class="p-button-secondary mr-2" @click="onClearClick" />
                    </template>
                </Toolbar>
            </div>
        </div>
    </div>
</template>
