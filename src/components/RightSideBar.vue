<template>
    <!--container-->
    <div class="container" ref="container">
        <nav class="navbar">
            <button class="navbars course" @click="showInfo(1)" :class="{ active: activeButton === 1 }">
                <span><i class="icon pi pi-book"></i>Course</span>
            </button>
            <button class="navbars room" @click="showInfo(2)" :class="{ active: activeButton === 2 }">
                <span><i class="icon pi pi-building"></i>Room</span>
            </button>
            <button class="navbars lecturer" @click="showInfo(3)" :class="{ active: activeButton === 3 }">
                <span><i class="icon pi pi-users"></i>Lecturer</span>
            </button>
        </nav>
        <!-- course  -->
        <div class="sub_container" v-if="activeButton === 1">
            <div class="search">
                <input type="text" placeholder="Search Subject .." v-model="course_search">
                <!-- <i class="icon pi pi-search"></i> -->
            </div>
            <div class="data">
                <div class="items">
                    <div class="itemss" v-for="course in filteredCourses" :key="course.id">
                        <!-- course -->
                        <div class="item" v-if="course.time_course != 0" draggable="true">
                            <input type="text" :data-course="JSON.stringify(course)"
                                :data-courseType="course.time_course != 0 ? 'Course' : ''" hidden>
                            <div class="icons">
                                <i class="icon pi pi-ellipsis-v"></i>
                                <i class="icon pi pi-ellipsis-v"></i>
                            </div>
                            <div class="course_info">
                                <div>
                                    <h1>{{ course.name_en }}</h1>
                                </div>
                                <div class="btn"><button>Unsigned</button></div>
                                <div>
                                    <p v-if="course.time_course != 0">Course : {{ course.time_course }} H</p>
                                </div>
                            </div>
                        </div>
                        <!-- TP -->
                        <div class="item" v-if="course.time_tp != 0" draggable="true">
                            <input type="text" :data-course="JSON.stringify(course)"
                                :data-courseType="course.time_tp != 0 ? 'TP' : ''" hidden>
                            <div class="icons">
                                <i class="icon pi pi-ellipsis-v"></i>
                                <i class="icon pi pi-ellipsis-v"></i>
                            </div>
                            <div class="course_info">
                                <div>
                                    <h1>{{ course.name_en }}</h1>
                                </div>
                                <div class="btn"><button>Unsigned</button></div>
                                <div>
                                    <p v-if="course.time_tp != 0">TP : {{ course.time_tp }} H</p>
                                </div>
                            </div>
                        </div>
                        <!-- TD -->
                        <div class="item" v-if="course.time_td != 0" draggable="true">
                            <input type="text" :data-course="JSON.stringify(course)"
                                :data-courseType="course.time_td != 0 ? 'TD' : ''" hidden>
                            <div class="icons">
                                <i class="icon pi pi-ellipsis-v"></i>
                                <i class="icon pi pi-ellipsis-v"></i>
                            </div>
                            <div class="course_info">
                                <div>
                                    <h1>{{ course.name_en }}</h1>
                                </div>
                                <div class="btn"><button>Unsigned</button></div>
                                <div>
                                    <p v-if="course.time_td != 0">TD : {{ course.time_td }} H</p>
                                </div>
                            </div>
                        </div>
                    </div>
                    <!-- <div class="see_more" v-if="course_search == ''">
                        <button @click="seeMore">see more..</button>
                    </div> -->
                    <div class="no_course"
                        v-if="(course_search != filteredCourses) && (course_search != '') && (filteredCourses.length == 0)">
                        <h3>No Course!</h3>
                    </div>
                </div>
            </div>
        </div>
        <!-- room  -->
        <div class="sub_container" v-if="activeButton === 2">
            <div class="search">
                <input type="text" placeholder="Search Room .." v-model="room_search">
                <!-- <i class="icon pi pi-search"></i> -->
            </div>
            <div class="data">
                <div class="items">
                    <div class="item-room" v-for="room in filteredRooms" :key="room.id" @click="checkRoom(room)">
                        <div class="room-number">
                            <h3>{{ room.building.code }}-{{ room.name }}</h3>
                        </div>
                        <div class="room-name">
                            <h4>{{ room.room_type.name }}</h4>
                            <!-- <p>{{ room.nb_desk }} Desk</p> -->
                            <p>
                                <span v-if="room.nb_desk === null">N/A</span>
                                <span v-else>{{ room.nb_desk }}</span> Desk
                            </p>
                            <p>
                                <span v-if="room.nb_chair === null">N/A</span>
                                <span v-else>{{ room.nb_chair }}</span> Chair
                            </p>
                        </div>
                    </div>
                    <div class="see_more" v-if="room_search == ''">
                        <button @click="seeMoreRoom">see more..</button>
                    </div>
                    <div class="no_room"
                        v-if="(room_search != filteredRooms) && (room_search != '') && (filteredRooms.length == 0)">
                        <h3>No Room</h3>
                    </div>
                </div>
            </div>
        </div>
        <!-- lecturer -->
        <div class="sub_container" v-if="activeButton === 3">
            <div class="search">
                <input type="text" placeholder="Search Lecturer .." v-model="lecturer_search">
                <!-- <i class="icon pi pi-search"></i> -->
            </div>
            <div class="data">
                <div class="items">
                    <div class="item-lecturer" v-for="lecturer in filteredLecturers" :key="lecturer.id"
                        @click="checkLecturer(lecturer)">
                        <div class="icon">
                            <img src="../assets/image/avatar.png" alt="avatar">
                        </div>
                        <div class="info">
                            <h4>
                                <span v-if="lecturer.id_card === null">No ID Card</span>
                                <span v-else>{{ lecturer.id_card }}</span>
                                | {{ lecturer.name_latin }}
                            </h4>
                            <p>{{ lecturer.name_latin }}</p>
                            <div class="depart-gen">
                                <div><i class="icon pi pi-home"></i>{{ lecturer.department.code }}</div>
                                <div><i class="icon pi pi-user"></i>{{ lecturer.gender.code }}</div>
                            </div>
                        </div>
                    </div>
                    <div class="see_more" v-if="lecturer_search == ''">
                        <button @click="seeMoreLecturer">see more..</button>
                    </div>
                    <div class="no_lecturer"
                        v-if="(lecturer_search != filteredLecturers) & (lecturer_search !== '') & (filteredLecturers.length == 0)">
                        <h3>No Lecturer</h3>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

import 'primeicons/primeicons.css';
import axios from 'axios';
// import interactionPlugin from '@fullcalendar/interaction'
import interactionPlugin, { Draggable } from '@fullcalendar/interaction';

export default {
    data() {
        return {
            activeButton: 1,
            rooms: [],
            lecturers: [],
            courses: [],
            course_search: '',
            room_search: '',
            lecturer_search: '',
            course_page: 1,
            container: undefined,
        }
    },
    watch: {
        selectedAcademyYear: function () {
            this.fetchCourses()
        },
        selectedDepartment: function () {
            this.fetchCourses();
        },
        selectedDegree: function () {
            this.fetchCourses();
        },
        selectedDepOption: function () {
            this.fetchCourses();
        },
        selectedGrade: function () {
            this.fetchCourses();
        },
        selectedSemester: function () {
            this.fetchCourses();
        }
    },
    props: [
        "selectedAcademyYear",
        "selectedDepartment",
        "selectedDegree",
        "selectedDepOption",
        "selectedGrade",
        "selectedSemester"
    ],
    methods: {
        checkRoom(room) {
            let room1 = room
            console.log(room1);
            // console.log(room1.name);
            // console.log(room1.building.code);
            console.log(room1.building.code, '-', room1.name);
            // console.log(this.rooms[0]);
        },
        checkLecturer(lecturer) {
            console.log(lecturer);
            console.log(lecturer.name_latin);
        },
        insertDataToApp(courseFake){
            console.log(courseFake)
            // this.$emit("insertDrag",)
        },
        showInfo(buttonNumber) {
            this.activeButton = buttonNumber;
        },
        seeMore() {
            this.course_page += 1;
            axios.get(`http://127.0.0.1:8000/api/course_annuals/get_course?page=${this.course_page}`)
                .then(response => this.courses = [...this.courses, ...response.data]);
        },
        seeMoreRoom() {
            this.course_page += 1;

            axios.get(`${import.meta.env.VITE_APP_ROOM}?page=${this.course_page}`)
                .then(response => this.rooms = [...this.rooms, ...response.data.data]);
        },
        seeMoreLecturer() {
            this.course_page += 1;

            axios.get(`${import.meta.env.VITE_APP_LECTURER}?page=${this.course_page}`)
                .then(response => this.lecturers = [...this.lecturers, ...response.data.data]);
        },
        fetchCourses() {
            axios.get(`http://127.0.0.1:8000/api/course_annuals/get_course` + "?" + new URLSearchParams({
                academic_year_id: this.selectedAcademyYear,
                department_id: this.selectedDepartment,
                degree_id: this.selectedDegree,
                department_option_id: this.selectedDepOption,
                grade_id: this.selectedGrade,
                semester_id: this.selectedSemester,
            }))
                .then((response) => {
                    this.courses = response.data
                    console.log(this.courses);
                })
                .catch((error) => {
                    console.log('Error fetching courses:', error);
                });
        }
    },
    mounted() {
        var containerEl = this.$refs.container;
            console.log(this.$refs);
            // console.log(containerEl);
                    
            var refreshCalendar=()=>{
                this.$emit("refreshCalendar",false);
            }
            new Draggable(containerEl, {
                itemSelector: '.item',
                eventData: function (eventEl) {
                    refreshCalendar();
                    return eventEl;
                }
            });
        axios.get(import.meta.env.VITE_APP_ROOM)
            .then(response => {
                this.rooms = response.data.data;
            });

        axios.get(import.meta.env.VITE_APP_LECTURER)
            .then(response => {
                this.lecturers = response.data.data;
            });
        this.fetchAcademyYears()
        this.selectedAcademyYear = this.selectedAcademyYear ?? this.fetchedAcademyYears[0]
    },
    computed: {
        filteredCourses() {
            const searchTerm = this.course_search.toLowerCase().trim();

            if (!searchTerm) {
                return this.courses;
            }

            return this.courses.filter(course => {
                return (
                    course.name_en.toLowerCase().includes(searchTerm) ||
                    (course.time_course != 0 && course.time_course.toString().includes(searchTerm)) ||
                    (course.time_tp != 0 && course.time_tp.toString().includes(searchTerm)) ||
                    (course.time_td != 0 && course.time_td.toString().includes(searchTerm))
                );
            });
        },
        filteredRooms() {
            const searchTerm = this.room_search.toLowerCase().trim();

            if (!searchTerm) {
                return this.rooms;
            }

            return this.rooms.filter(room => {
                const fullRoomName = `${room.building.code}-${room.name}`.toLowerCase();

                return (
                    fullRoomName.includes(searchTerm) ||
                    room.building.code.toLowerCase().includes(searchTerm) ||
                    room.room_type.name.toLowerCase().includes(searchTerm) ||
                    (room.nb_desk !== null && room.nb_desk.toString().includes(searchTerm)) ||
                    (room.nb_chair !== null && room.nb_chair.toString().includes(searchTerm))
                );
            })
        },
        filteredLecturers() {
            const searchTerm = this.lecturer_search.toLowerCase().trim();

            if (!searchTerm) {
                return this.lecturers;
            }

            return this.lecturers.filter(lecturer => {
                const idCard = `${lecturer.id_card}`;
                const lecturerNameLatin = lecturer.name_latin.toLowerCase();

                return (
                    idCard.includes(searchTerm) ||
                    lecturerNameLatin.includes(searchTerm)
                );
            });
        },
    }
}
</script>

<style scoped>
/* navbar button color change */
button.navbars.course.active,
button.navbars.room.active,
button.navbars.lecturer.active {
    background-color: #0c356a;
}
button.navbars.course, button.navbars.room, button.navbars.lecturer{
    cursor: pointer;
}

/* container */
.container {
    width: 20%;
    height: 94.6vh;
    /* background-color: rgb(190, 190, 190); */
    font-family: Arial, Helvetica, sans-serif;
    padding-right: 7px;
}

/* navbar */
.container .navbar {
    height: 35px;
    display: flex;
    position: relative;
}

.container .navbar .navbars {
    width: 84px;
    height: 35px;
    border: none;
    border-radius: 5px;
    background-color: #3aa6b9;
    color: white;
    position: absolute;
    transition: all 0.6s;
}

.container .navbar .course {
    top: 17px;
}

.container .navbar .room {
    top: 17px;
    left: 92px;
}

.container .navbar .lecturer {
    top: 17px;
    left: 184px;
}

.container .navbar .navbars:hover {
    background-color: #0c356a;
    transition: all 0.6s;
}

.container .navbar .navbars span {
    font-size: 15px;
}

.container .navbar .navbars span .icon {
    padding-right: 9px;
}

/* .sub_container */
.container .sub_container {
    width: 100%;
    height: 95%;
    margin-top: 2.2rem;
}

/* .search */
.container .sub_container .search {
    width: 100%;
    height: 35px;
    /* background-color: aqua; */
    display: flex;
    justify-content: center;
    align-items: center;
    /* border: 1px solid black; */
    margin-top: 35px;
    margin-bottom: 10px;
    outline: 0px;
}

.container .sub_container .search input {
    width: 100%;
    padding: .4rem;
    outline: none;
    position: relative;
    font-size: 16px;
    /* background-color: #EEEEEE; */
    border: 1px solid #B2B2B2;
    border-radius: 3px;
}

.container .sub_container .search input::placeholder {
    font-size: 15px;
    padding-left: 4px;
    font-weight: medium;
    color: #9DB2BF;
}

.container .sub_container .data {
    width: 100%;
    height: 94%;
    background-color: rgb(255, 255, 255);
    border: 1px solid #B2B2B2;
    border-radius: 3px;
}

.container .sub_container .data .items {
    width: 95%;
    height: 95%;
    background-color: #ece8e8;
    /* padding: 5%; */
    margin: 3%;
    overflow-y: scroll;
    border-radius: 3px;
}

/* course only*/
.container .sub_container .data .items .itemss {
    width: 100%;
}

/* item course  */
.container .sub_container .data .items .itemss .item {
    height: 100px;
    /* width: 100%; */
    background-color: #ffffff;
    /* padding: 5%; */
    margin: 4.5%;
    /* transition: all 0.4s; */
    border-left: 2px solid #ff0000;
    border-radius: 3px;
    cursor: move;
    /* user-select: none; */
    /* touch-action: manipulation; */
}

.container .sub_container .data .items .item:hover {
    background-color: #A7ECEE;
    box-shadow: -2px 2px 6px -4px rgba(0, 0, 0, 0.79);
    /* cursor: move; */
    transition: all 0.4s;
}

.container .sub_container .data .items .item .icons {
    width: 100%;
    height: 20%;
    /* background-color: aliceblue; */
    font-size: 12px;
    display: flex;
    align-items: center;
    padding: 12px 0px 12px 6px;
}

.container .sub_container .data .items .item .course_info {
    height: 80%;
    width: 100%;
    background-color: aliceblue;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    padding-left: 5%;
    border-radius: 3px;
}

.container .sub_container .data .items .item .course_info h1 {
    font-size: 15px;
}

.container .sub_container .data .items .item .course_info .btn button {
    color: white;
    background-color: #ff0000;
    border: none;
    border-radius: 5px;
    padding: .2rem;
    font-size: 12px;
}

/* item-room */
.container .sub_container .data .items .item-room {
    height: 83px;
    background-color: #ffffff;
    margin: 5%;
    /* border-left: 2px solid #ff0000; */
    display: flex;
    transition: all 0.4s;
    border-radius: 3px;
}

.container .sub_container .data .items .item-room:hover {
    background-color: #ECF2FF;
    box-shadow: -2px 2px 6px -4px rgba(0, 0, 0, 0.75);
    cursor: pointer;
    transition: all 0.4s;
}

.container .sub_container .data .items .item-room .room-number {
    width: 30%;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #71dcfe;
    color: white;
    border-top-left-radius: 3px;
    border-bottom-left-radius: 3px;
}

.container .sub_container .data .items .item-room .room-number h4 {
    font-size: 21px;
}

.container .sub_container .data .items .item-room .room-name {
    width: 70%;
    display: flex;
    flex-direction: column;
    justify-content: start;
    padding-top: 0.45rem;
    padding-left: .5rem;
}

.container .sub_container .data .items .item-room .room-name h3 {
    font-size: 18px;
    padding-bottom: 4.60px;
}

.container .sub_container .data .items .item-room .room-name p {
    font-size: 12px;
}

/* item-lecturer */
.container .sub_container .data .items .item-lecturer {
    height: 85px;
    background-color: #ffffff;
    border-left: 2px solid #002aff;
    border-radius: 3px;
    display: flex;
    margin: 4.5%;
    transition: all 0.5s;
}

.container .sub_container .data .items .item-lecturer:hover {
    background-color: #ECF2FF;
    box-shadow: -2px 2px 6px -4px rgba(0, 0, 0, 0.75);
    cursor: pointer;
    transition: all 0.5s;
}

.container .sub_container .data .items .item-lecturer .icon {
    width: 20%;
    height: 100%;
    display: flex;
    align-items: center;
}

.container .sub_container .data .items .item-lecturer .icon img {
    width: 100%;
    object-fit: cover;
}

.container .sub_container .data .items .item-lecturer .info {
    width: 80%;
    padding-top: 0.57rem;
    padding-left: .5rem;
}

.container .sub_container .data .items .item-lecturer .info h4 {
    font-size: 16px;
    padding-bottom: 5px;
}

.container .sub_container .data .items .item-lecturer .info p {
    font-size: 14px;
}

.container .sub_container .data .items .item-lecturer .info .depart-gen {
    width: 100%;
    display: flex;
    /* gap: .5rem; */
}

.container .sub_container .data .items .item-lecturer .info .depart-gen div {
    width: 50%;
    display: flex;
    font-size: 12px;
    padding-top: 9.3px;
}



/*see_more and no_course */
.see_more,
.no_course,
.no_room,
.no_lecturer {
    display: flex;
    justify-content: center;
    align-items: center;
    border-left: none;

    height: 100px;
    /* width: 100%; */
    background-color: #ffffff;
    /* padding: 5%; */
    margin: 4.5%;
    /* transition: all 0.4s; */
    /* border-left: 2px solid #ff0000; */
    border-radius: 3px;
    /* cursor: move; */
}

.see_more button {
    padding: .25rem .5rem;
    border: none;
    color: black;
    /* background-color: #0c356a; */
    z-index: 1;
    border-radius: .3rem;
}

.see_more button:hover {
    color: white;
    background-color: #3aa6b9;
    cursor: pointer;
}

@media screen and (min-width: 1369px){
    .contianer .sub-container .data .items .item-room{
        font-family: Arial, Helvetica, sans-serif;
    }
    .contianer .sub-container .data .items .item-room .room-name h3 {
        font-size: 18.5px;
    }
    .contianer .sub-container .data .items .item-room .room-name p {
        font-size: 13px;
    }

    .contianer .sub-container .data .items .item-lecturer .info p {
        font-size: 13px;
    }
}
</style>