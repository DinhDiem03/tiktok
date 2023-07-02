// actions

import {SET_JOB,ADD_JOB,DELETE_JOB,DETAIL_JOB,UPDATE_JOB} from "./constant"

export const setJob = (payload) => {
    return {
      type: SET_JOB,
      payload,
    };
  };
  export const addJob = (payload) => {
    return {
      type: ADD_JOB,
      payload,
    };
  };
  export const deleteJob = (payload) => {
    return {
      type: DELETE_JOB,
      payload,
    };
  };
  export const detailJob = (payload) => {
    return {
      type: DETAIL_JOB,
      payload,
    };
  };
  export const updateJob = (payload) => {
    return {
      type: UPDATE_JOB,
      payload,
    };
  };

  //constants
  export const SET_JOB = "set_job";
export const ADD_JOB = "add_job";
export const DELETE_JOB = "delete_job";
export const DETAIL_JOB = "detail_job";
export const UPDATE_JOB = "update_job";

// logger
function logger(reducer) {
  return (prevState, action) => {
    console.group(action.type);
    console.log("Prev state:", prevState);
    console.log('Action:', action);

    const newState = reducer(prevState, action);
    
    console.log("New state:", newState);
    console.groupEnd();
    return newState;
  };
}
export default logger;


// reducer

import {SET_JOB,ADD_JOB,DELETE_JOB,DETAIL_JOB,UPDATE_JOB} from "./constant"


export const initState = {
    job: "",
    jobs: [],
  };
  // Reducer
  const reducer = (state, action) => {


    // let index = action.payload;
    switch (action.type) {
      case SET_JOB:
        return {
          ...state,
          job: action.payload,
        };
  
      case ADD_JOB:
        return {
          ...state,
          jobs: [...state.jobs, action.payload],
        };
  
      case DELETE_JOB:
        const newJob = [...state.jobs];
        newJob.splice(action.payload, 1);
        return {
          ...state,
          jobs: newJob,
        };
      case DETAIL_JOB:
        const index = action.payload
        const jobDetail = state.jobs[index];
        return {
          
          ...state,
          job: jobDetail,
          jobIndex: index
        };
      case UPDATE_JOB:
        const updateJob =[...state.jobs]
          updateJob[state.jobIndex] = action.payload
         
        return {
          ...state,        
           jobs: updateJob,
          jobIndex: null
        };
  
      default:
        throw new Error("Invalid action");
    }
  };

  export default reducer

  // index

  import { useRef } from "react";
import { useReducer } from "react";
import reducer ,{initState} from "./reducer";
import { addJob,deleteJob,detailJob,setJob,updateJob } from "./actions";
import logger from "./logger";

function Reducer() {
  const [state, dispatch] = useReducer(logger(reducer), initState);
  const { job, jobs } = state;

  const inRef = useRef();

  const handleSubmit = () => {
    dispatch(addJob(job));
    dispatch(setJob(""));

    inRef.current.focus();
  };
  const handleUpdate = () => {
    dispatch(updateJob(job));
    dispatch(setJob(""));
   
  };

  return (
    <div>
      <input
        ref={inRef}
        value={job}
        placeholder="Enter todo..."
        onChange={(e) => {
          dispatch(setJob(e.target.value));
        }}
      />
      <button onClick={handleSubmit}>Add</button>
      <button onClick={handleUpdate}>Update</button>

      <ul>
        {jobs.map((item, index) => (
          <li key={index}>
            {item}
            <button onClick={() => dispatch(deleteJob(index))}>
             
              &times;
            </button>
            <button onClick={() => dispatch(detailJob(index))}> Detail </button>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default Reducer;

