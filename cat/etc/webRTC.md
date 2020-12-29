### WebRTC & server(vue.js, node.js)
내가 이해할정도만 정리.. 
삭제된 부분이 많다.. 


```
//뷰에는 비디오 화면 넣을 수 있는 비디오 태그 p1_video, p2_video존재


<script>
import io from "socket.io-client";

//스턴서버 연결
const pcConfig = {
  iceServers: [
    {
      urls: "stun:" + stun_server
    }
  ]
};
```

데이터 초기화 - 비디오, 스트림, pc를 배열로 
```
  data() {
    return {
      //video, stream
      player_videos:[null,null],
      player_streams:[null,null],
      player_idx:0,


      //대결하는 사람들의 정보
      battle_id:[null,null],
      battle_connections:[null,null],


      //시청자들은 가지고 있을 필요가 없음
      watchers_id: [null, null, null, null, null],
      watchers_connections: [null, null, null, null, null],
      watcher_cnt :0,

      room_id: 1,
      roomInfo: "",
      singer_num: 0,

      user_id: "hajung",
      my_position: "user1", //watcher는 없음
      user_identification: "watcher",

    };
  },
 ``` 
 메서드
  ```

    sendMessage(message) {
      //서버로 메세지 보내기(emit과 on-받기)
      this.socket.emit("message", message);
    },
    
    //본인의 영상 띄우는건 간단
    async get_stream(stream) {
      this.player_videos[this.player_idx].srcObject = stream;
      this.player_streams[this.player_idx] = stream;
    },
    
    //여기가 pc를 연결하는 코드
    //새로운 사용자가 들어왔을 때, join을 브로드캐스트 한다
    //기존 사용자는 join메세지를 받으면 offer(ice, sdp, stream)을 보낸다.
    //새로운 사용자는 다시 기존 사용자의 offer를 받고 answer을 전달.
    //각 과정에서 연결자인 icecandidate 설정이 들어간다.
    async getPeerConnection(전달 데이터) {
        let t_pc = await new RTCPeerConnection(pcConfig);
       //프로젝트 특성상 2가지 사용군이 존재하기 때문에 각 사용자군을 조건문 처리 하는것이 복잡... 
       // 다음이 대표적 코드
          bat_id = (this.player_idx+1)%2;
          this.battle_connections[bat_id] = t_pc;
          this.battle_id[bat_id] = join_id;
        
      t_pc.onicecandidate = event => {
        if (event.candidate) {
          this.sendMessage({
            message: {
              type: "candidate",
              label: event.candidate.sdpMLineIndex,
              id: event.candidate.sdpMid,
              candidate: event.candidate.candidate
            },
            //전달할 데이터 추가
          });
        }
      };
        //연결된 피어의 이벤트 발생시 이벤트 스트림 저장
      t_pc.onaddstream = event => {
        this.player_streams[bat_id] = event.stream;

        this.player_videos[bat_id].srcObject = this.player_streams[bat_id];
        
      }
        //본인의 스트림을 저장하여 리턴해준다.
        t_pc.addStream(this.player_streams[this.player_idx]);
      
      return t_pc;
    }
  },
  
  //이부분에서 소켓을 시작하도록 했는데 새로고침할때마다 소켓이 새로 생성된다... 고쳐야될 부분
  created() {
  //여기서 소켓 생성~~~~~~~~~~~~
  },
  mounted() {
  
    this.player_videos[0] = document.getElementById("p1_video");
    this.player_videos[1] = document.getElementById("p2_video");
    
    //player1일때,
    if (this.user_identification == "singer") {
      console.log("가수");
      navigator.mediaDevices
        .getUserMedia({
          audio: true,
          video: true
        })
        .then(this.get_stream);

      // if (this.my_position=="user1"){

      // }
    }
    
    //새로운 상대가 들어온것을 join메세지로 전달 받는다.
    this.socket.on("join", message => {
      const join_id = message.user_id;
      const join_identification = message.user_identification;
      const join_idx = message.player_idx;
      //에러가 떠서 타임을 설정해줌... 이유를 찾아볻자...
        setTimeout(() => {
          this.getPeerConnection().then(t_pc => {
            t_pc.createOffer(
              sdp => {
                t_pc.setLocalDescription(sdp);
                this.sendMessage({
                  //전송 데이터 추가
                  //sdp전달하는 코드임 offer생성
                });
              },
              e => {
                console.log(e);
              }
            );
          });
        }, 1000);
        
    this.socket.on("leave", message => {
     //나갈때 메세지 전달...
     //보통 변수를 null로 초기화해준다.
     
     
    });
    
    //여기서 offer, answer, candidate 메세지를 받고 처리 해준다...
    this.socket.on("message", data => {
      if (data.message.type == "offer") {
          //내가 새로 들어왔을 때, join 메세지를 보내면,
          //기존의 사용자들이 나의 join을 받고 
          //자신의 정보를 담은 offer를 보낸다.
        if (this.user_identification=="singer"){
            //내가 가수이고
          if (data.from_identification=="singer"){
              //상대도 가수
              console.log("오퍼를 받앗습니다.답장을 해요",data);
              this.battle_id[(this.player_idx+1)%2] = data.from;
          }else{
            //상대가 시청자일 경우
            this.watchers_id[this.watcher_cnt++] = data.from;
          }

          this.getPeerConnection().then(t_pc => {
                
                t_pc.setRemoteDescription(new RTCSessionDescription(data.message));
                t_pc.createAnswer().then(sdp => {
                    t_pc.setLocalDescription(sdp);
                    this.sendMessage({
                    });
                });
            });
        }  
            
        
      } else if (data.message.type == "answer") {
         
        let t_pc = null;
            t_pc = this.battle_connections[(this.player_idx+1)%2]; 
              t_pc = this.battle_connections[data.from_idx]; 
          
        }
        
        t_pc.setRemoteDescription(new RTCSessionDescription(data.message));
      } else if (data.message.type === "candidate") {
        let candidate = new RTCIceCandidate({
          sdpMLineIndex: data.message.label,
          candidate: data.message.candidate
        });
        // //let t_pc = this.peer_connection;
        // console.log("여기 캔디데이트", cand);
        // console.log("여기는 pc?", this.op_connection);
        let t_pc = null;
         
        t_pc = this.battle_connections[(this.player_idx+1)%2]; 
        t_pc.addIceCandidate(candidate);
         }
        
        
      }
    });
  }
};
</script>
```

#### 서버단
```
//룸을 이렇게 처리 해야되더라.. 수정이 가능하다면 하고 싶음..
let rooms = [null,null,null,null,null];

server.listen(port, function() {
    console.log('socket io server listening on port '+port);
})

//connection event handler
io.on('connection' , function(socket) {

    console.log('Connect from Client: '+ socket.id)

    console.log(socket.handshake.query);
    let room_id = socket.handshake.query.room_id;
    let user_id = socket.handshake.query.user_id;
    let user_identification = socket.handshake.query.user_identification;
    let player_idx = socket.handshake.query.player_idx;
    let room = rooms[room_id];
    //let watcher_cnt = 0;

    let watchers = [null,null,null,null,null];

    if(room){
        //방이 존재 할때
        if (user_identification=="singer"){
            console.log(room.singer1)
            //노래 부르는 사람일때
            if (room.singer1){
                console.log("여기는 싱어1이 있을때 ")
                room.singer2=user_id;
            }else {
                room.singer1=user_id;
            }
        }else if (user_identification=="watcher"){//시청자일 때,
            
            if (room.watcher_cnt>=6){
              return;
            }
            
            room.watchers[room.watcher_cnt] = user_id;
            room.watcher_cnt++;
            console.log(room.watcher_cnt);
            console.log(room.watchers);
            //room.watcher_cnt += 1;
        }
        room.sockets.push(socket);

    } else{
        //방이 존재하지 않을 때,
        console.log("방이 존재x")
        if (user_identification=="singer"){
            room = {
                singer1: user_id,
                singer2: null,
                watcher_cnt: 0,
                watchers: {},
                sockets:[socket]
            }   
        }else{
            //시청자일 때,
            room = {
                singer1: null,
                singer2: null,
                watcher_cnt: 1,
                watchers: [user_id],
                sockets:[socket]
            }   
        }
            rooms[room_id] = room;
    }
    socket.join(room_id);
    console.log(rooms);

    socket.to(room_id).broadcast.emit('join',{
        user_id: user_id,
        user_identification: user_identification,
        watcher_cnt: room.watcher_cnt,
        watchers: watchers,
        player_idx: player_idx
    });


    //webRTC
    socket.on('message', data =>{
       console.log("message in", data.message.type);
       
        
        socket.broadcast.emit('message', data);
    });


})
```
