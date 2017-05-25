import React from 'react'
import {Card, Row, Col, Tree, TreeNode, Tag} from 'antd';
import GoogleMapReact from 'google-map-react';

const structure = [
{
	name: 'Алматы',
		branches: [
					{title: 'ПЛ. РЕСПУБЛИКИ, 15'},
					{title: 'УЛ. КАРАСАЙ БАТЫРА, 98'}
				],
		atms: [
				{title: 'УЛ. ФУРМАНОВА, 43/171. VIP-ЦЕНТР',  lat: '43.244833', long: '76.946806', state:'on'} ,
				 {title:  'УЛ. ТОЛЕ БИ, 55, УГОЛ УЛ. ПАНФИЛОВА (ОФИС БАНКА)', lat:"43.255167", long: "76.945500", state: "on"} ,
				 {title:  'ПЛ. РЕСПУБЛИКИ, 13 УГОЛ УЛ. ФУРМАНОВА (ОФИС БАНКА)',  lat: "43.238445", long: "76.943363", state:"on"}
			]
},
{
	name: 'Астана',
		branches: [
					{title: 'УЛ. ДОСТЫК, 5 (МЖК «СЕВЕРНОЕ СИЯНИЕ» ПОМЕЩЕНИЕ ВП-91)'} ,
					{title: 'ПР. РЕСПУБЛИКИ, 10.'}
				],
		atms: [
				{title: 'УЛ. ДОСТЫК, 5 (КОМПЛЕКС «СЕВЕРНОЕ СИЯНИЕ»)', lat: "51.145861", long: "71.413056", state: "on"} ,
				 {title: 'КОРГАЛДЖИНСКОЕ ШОССЕ, 1. ТРЦ "MEGA ASTANA"', lat: "51.145861", long: "71.413056", state: "on"}
				]
}

];

const someProps = {
    center: {lat: 59.95, lng: 30.33},
    zoom: 11
  };
  const AnyReactComponent = ({ text }) =>  <Tag color="#2db7f5">{text}</Tag>;

const MapComponent = () => (
  <div className="offices-block">
    <h3>This is map</h3>
    <Row >
       <Col span={8}>
         <Tree>
            {
              structure.map( (item, index) =>
                 <Tree.TreeNode  key={index} title={item.name}>
                    {
                      item.branches.map( (branch, ind) =>
                        <Tree.TreeNode  key={ind+10} title={"Отделение: " + branch.title}/>
                      )
                    }
                    { item.atms.map( (atm, ind2) =>
                        <Tree.TreeNode  key={ind2+20} title={"ATM: " + atm.title}/>
                      )
                    }

                 </Tree.TreeNode>
               )
            }

         </Tree>
       </Col>
       <Col span={16}>

             <GoogleMapReact
             bootstrapURLKeys={{key: 'AIzaSyCw7ulBeA1QP_c8lT_Q2MG3XylciUZVfdU'}}
             apiKey='AIzaSyCw7ulBeA1QP_c8lT_Q2MG3XylciUZVfdU'
          defaultCenter={someProps.center}
          defaultZoom={someProps.zoom}
        >
          <AnyReactComponent
            lat={59.955413}
            lng={30.337844}
            text={'Kreyser Avrora'}
          />
        </GoogleMapReact>
       </Col>
     </Row>
   </div>
);

export default MapComponent;



import React from 'react'
import { Calendar, Alert, Table} from 'antd';
import moment from 'moment';
import 'moment/locale/ru';
moment.locale('ru');

class LeftSideComponent extends React.Component {
  state = {
    value: moment(),
    selectedValue: moment(),
    events: []
  }
  onSelect = (value) => {
    const events = [];
    if(value.month()==4){
      switch (value.date()) {
        case 1:
          events.push({work: false, title: 'Праздник единства народа Казахстана'});
          break;

        case 7:
          events.push( {work: false, title: 'День защитника Отечества'});
          break;
          case 9:
            events.push({work: false, title: 'День Победы'});
            break;
        default:

          break;
      }

    }
    this.setState({
      value,
      selectedValue: value,
      events: events
    });
  }
  onPanelChange = (value) => {
    this.setState({ value });
  }

  render() {
    const { value, selectedValue } = this.state;
    const columns = [{
        title: null,
        dataIndex: 'title',
        key: 'title',
      }];
    return (
      <div>
        <div style={{ width: 250, border: '1px solid #d9d9d9', borderRadius: 4 }}>
            <Calendar value={value} onSelect={this.onSelect} onPanelChange={this.onPanelChange} fullscreen={false}/>
        </div>
        <Alert style={{ width: 250, border: '1px solid #d9d9d9', borderRadius: 4 }} message={`Вы выбрали: ${selectedValue && selectedValue.format('YYYY-MM-DD')}`} />
        <Table dataSource={this.state.events} columns={columns}  pagination={false}/>
      </div>
    );
  }
}

export default LeftSideComponent;
