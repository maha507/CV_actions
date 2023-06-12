<script>
	import { onMount } from "svelte";
	import "bootstrap/dist/css/bootstrap.min.css";
	import DevExpress from "devextreme";
	import UploadCV from "./UploadCV.svelte";
	let jsonData = [];
	let gridData = [];
  
	async function handleFileUpload(event) {
	  const file = event.target.files[0];
	  // Perform further actions with the uploaded file
  
	  // Example: Update the backend API URL with the file upload
	  const formData = new FormData();
	  formData.append("file", file);
  
	  fetch(
		`https://api.recruitly.io/api/candidatecv/upload?apiKey=TEST1236C4CF23E6921C41429A6E1D546AC9535E&candidateId=${uploadCandidateId}`,
		{
		  method: "POST",
		  body: formData,
		}
	  )
		.then((response) => {
		  // Handle the response accordingly
		  if (response.ok) {
			console.log("CV uploaded successfully!");
		  } else {
			console.error("CV upload failed.");
		  }
		})
		.catch((error) => {
		  console.error("CV upload error:", error);
		});
	}
  
	onMount(async () => {
	  const response = await fetch(
		"https://api.recruitly.io/api/candidate?apiKey=TEST9349C0221517DA4942E39B5DF18C68CDA154"
	  );
	  const responseData = await response.json();
	  jsonData = responseData.data;
  
	  gridData = jsonData.map((item) => ({
		id: item.id,
		firstName: item.firstName,
		surname: item.surname,
		email: item.email,
		mobile: item.mobile,
		cvUrl: item.cvUrl, // assuming cvUrl is the property containing the CV file URL
	  }));
  
	  const dataGrid = new DevExpress.ui.dxDataGrid(
		document.getElementById("dataGrid"),
		{
		  dataSource: gridData,
		  columns: [
			{ dataField: "id", caption: "ID", width: 50 },
			{ dataField: "firstName", caption: "First Name", width: 200 },
			{ dataField: "surname", caption: "Surname", width: 200 },
			{ dataField: "email", caption: "Email", width: 200 },
			{ dataField: "mobile", caption: "Mobile", width: 150 },
			{
			  caption: "CV Actions",
			  width: 400,
			  cellTemplate: function (container, options) {
				const cvUploadButton = document.createElement("input");
				cvUploadButton.type = "file";
				cvUploadButton.accept = "application/pdf"; // specify accepted file type if required
				cvUploadButton.addEventListener("change", handleFileUpload);
  
				const cvDownloadButton = document.createElement("button");
				cvDownloadButton.innerText = "CV Download";
				cvDownloadButton.classList.add("btn", "btn-info", "mr-2");
				cvDownloadButton.addEventListener("click", function () {
				  const rowData = options.data;
				  // Implement CV download logic here
				  console.log("CV Download clicked for row:", rowData);
				});
  
				const viewCVButton = document.createElement("button");
				viewCVButton.innerText = "View CV";
				viewCVButton.classList.add("btn", "btn-secondary");
				viewCVButton.addEventListener("click", function () {
				  const rowData = options.data;
				  // Implement view CV logic here
				  console.log("View CV clicked for row:", rowData);
				});
  
				container.appendChild(cvUploadButton);
				container.appendChild(cvDownloadButton);
				container.appendChild(viewCVButton);
			  },
			},
		  ],
		  showBorders: true,
		  filterRow: {
			visible: true,
		  },
		  editing: {
			allowDeleting: true,
			allowAdding: true,
			allowUpdating: true,
			mode: "popup",
			form: {
			  labelLocation: "top",
			},
			popup: {
			  showTitle: true,
			  title: "Row in the editing state",
			},
			texts: {
			  saveRowChanges: "Save",
			  cancelRowChanges: "Cancel",
			  deleteRow: "Delete",
			  confirmDeleteMessage:
				"Are you sure you want to delete this record?",
			},
		  },
		  paging: {
			pageSize: 20,
		  },
		  onRowInserting: async (e) => {
			// ...
		  },
		  onRowUpdating: async (e) => {
			// ...
		  },
		  onRowRemoving: async (e) => {
			// ...
		  },
		  onInitialized: () => {
			// Function called when the grid is initialized
			// ...
		  },
		}
	  );
	});
  </script>
  
  <div id="dataGrid"></div>
  