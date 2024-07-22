<script setup>
import { ref } from 'vue';
import Menubar from './MenuBar.vue';
import UpperTable from './UpperTable.vue';
import LowerTable from './LowerTable.vue';
import axios from 'axios';

// 상단 그리드에 표시될 데이터
const upperTableData = ref([]);
// 하단 그리드에 표시될 데이터
const lowerTableData = ref([]);
// 로딩 상태 관리 (그리드마다 별도 컨트롤)
const loading = ref(false);

// '/data.json'에서 json 데이터 가져와서 updateUpperData 이벤트 발생시키는 로직 -> 백엔드 연동 수정 완
const displayCompanyData = async () => {
    loading.value = true;
    try {
        // 정적 파일 data.json 데이터 요청
        // const res = await axios.get('/data.json');
        // json 서버에 데이터 요청
        // const res = await axios.get('http://localhost:3000/customers')
        // 백엔드에 데이터 요청
        const res = await axios.get('http://localhost:8080/my-page/companies');
        // console.log('조회 성공', data.value);
        upperTableData.value = res.data.companies;
    } catch (error) {
        console.error('데이터 가져오는 중 오류 발생:', error);
    } finally {
        loading.value = false;
    }
};

const displayUserData = async () => {
    loading.value = true;
    try {
        const res = await axios.get('http://localhost:8080/my-page/users');
        // console.log('조회 성공', data.value);
        lowerTableData.value = res.data.users;
    } catch (error) {
        console.error('데이터 가져오는 중 오류 발생:', error);
    } finally {
        loading.value = false;
    }
};

// 상, 하단 그리드 데이터 초기화 로직
const clearData = () => {
    upperTableData.value = [];
    lowerTableData.value = [];
    // console.log('데이터 초기화');
};

// 검색 조건 null로 초기화
const resetSearchQuery = (searchQuery) => {
    searchQuery.companyId = null;
    searchQuery.companyName = null;
    searchQuery.country = null;
};

// 특정 행이 클릭되었을 때 해당 행 데이터를 emit으로 MyPage에 전달
const updateLowerTable = async (row) => {
    // console.log('선택된 행 데이터:', row.data);
    // console.log('선택된 행 ID:', row.data.companyId);
    loading.value = true;
    try {
        const res = await axios.get(`http://localhost:8080/my-page/companies/${row.data.companyId}`);
        console.log(res.data);
        lowerTableData.value = res.data.userDetails;
    } catch (error) {
        console.log('데이터 가져오는 중 오류 발생: ', error);
    } finally {
        loading.value = false;
    }
};

// 상단 그리드에서 검색 이벤트 처리
const handleSearch = async (searchQuery) => {
    console.log('검색 조건:', searchQuery);
    //loading.value = true;
    try {
        const res = await axios.get('http://localhost:8080/my-page/companies/search', {
            params: {
                companyId: searchQuery.companyId,
                companyName: searchQuery.companyName,
                country: searchQuery.country
            }
        });
        console.log('검색한 데이터:', res.data);
        upperTableData.value = res.data;
        resetSearchQuery(searchQuery);
    } catch (error) {
        console.error('데이터 가져오는 중 오류 발생:', error);
    } finally {
        //loading.value = false;
    }
};
</script>

<template>
    <div>
        <h1>My Table</h1>
        <Menubar @display-company-data="displayCompanyData" @display-user-data="displayUserData" @clearData="clearData" />
        <UpperTable :data="upperTableData" @row-click="updateLowerTable" @search="handleSearch" />
        <LowerTable :data="lowerTableData" :loading="loading" />
    </div>
</template>

<style scoped></style>
