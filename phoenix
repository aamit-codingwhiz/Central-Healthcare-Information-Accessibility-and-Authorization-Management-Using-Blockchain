/* SPDX-License-Identifier: UNLICENSED */

pragma solidity ^0.8.17;

contract PatientHistory {
    struct Patient {
        string id;
        string[] centerIds;
        string[] testReports;
    }
    Patient[] private patients;
    string[] private patientIds;
    uint256 public countPatient = 0;

    function addPatient(string memory _id) public {
        require(!isPatientRegistered(_id), "Patient ID already exists.");

        patients.push(Patient(_id, new string[](0), new string[](0)));
        patientIds.push(_id);
        countPatient++;
    }

    function removePatient(string memory _patientId) public {
        require(isPatientRegistered(_patientId), "Patient ID does not exist.");
        uint256 patientIndex = getPatientIdIndex(_patientId);
        delete patients[patientIndex];
    }

    function isPatientRegistered(string memory _id) public view returns (bool) {
        for (uint256 i = 0; i < countPatient; i++) {
            if (
                keccak256(abi.encodePacked(patientIds[i])) ==
                keccak256(abi.encodePacked(_id))
            ) {
                return true;
            }
        }
        return false;
    }

    function getPatientIdIndex(string memory _id)
        private
        view
        returns (uint256)
    {
        require(isPatientRegistered(_id), "Patient ID do not exist.");

        for (uint256 i = 0; i < countPatient; i++) {
            if (
                keccak256(abi.encodePacked(patientIds[i])) ==
                keccak256(abi.encodePacked(_id))
            ) {
                return i;
            }
        }
        return uint256(0);
    }

    function addTestReport(
        string memory _patientId,
        string memory _centerId,
        string memory _report
    ) public {
        require(isPatientRegistered(_patientId), "Patient ID do not exist.");
        uint256 patientIndex = getPatientIdIndex(_patientId);

        Patient storage patient = patients[patientIndex];
        patient.centerIds.push(_centerId);
        patient.testReports.push(_report);
    }

    function getPatientHistory(string memory _patientId)
        public
        view
        returns (string[] memory centerIds, string[] memory testReports)
    {
        require(isPatientRegistered(_patientId), "Patient ID do not exist.");
        uint256 patientIndex = getPatientIdIndex(_patientId);

        Patient storage patient = patients[patientIndex];
        centerIds = patient.centerIds;
        testReports = patient.testReports;
    }

    function updateTestReport(
        string memory _patientId,
        string memory _newReport
    ) public {
        require(isPatientRegistered(_patientId), "Patient ID does not exist.");
        // require(isCenterAuthorized(_centerId), "Center is not authorized.");
        uint256 patientIndex = getPatientIdIndex(_patientId);

        Patient storage patient = patients[patientIndex];
        // require(patient.centerIds[_index] == _centerId, "Center does not have permission to update this report.");
        patient.testReports[0] = _newReport;
    }

    function removeTestReport(string memory _patientId) public {
        require(isPatientRegistered(_patientId), "Patient ID does not exist.");
        // require(isCenterAuthorized(_centerId), "Center is not authorized.");
        uint256 patientIndex = getPatientIdIndex(_patientId);

        Patient storage patient = patients[patientIndex];
        // require(patient.centerIds[_index] == _centerId, "Center does not have permission to delete this report.");
        // delete patient.centerIds[_index];
        delete patient.testReports[0];
    }

    function getRecentPatientReports(string memory _patientId, uint256 _count)
        public
        view
        returns (string[] memory)
    {
        require(isPatientRegistered(_patientId), "Patient ID does not exist.");
        uint256 patientIndex = getPatientIdIndex(_patientId);
        Patient storage patient = patients[patientIndex];

        // check if the count is greater than the length of the testReports array
        if (_count > patient.testReports.length) {
            _count = patient.testReports.length;
        }

        // create a new array to store the recent test reports
        string[] memory recentReports = new string[](_count);

        // iterate through the testReports array starting from the end and add the most recent test reports to the recentReports array
        for (uint256 i = 0; i < _count; i++) {
            recentReports[i] = patient.testReports[
                patient.testReports.length - 1 - i
            ];
        }

        return recentReports;
    }

    function getCountOfPatientsTestedByCenter(string memory _centerId)
        public
        view
        returns (uint256)
    {
        uint256 count = 0;
        for (uint256 i = 0; i < patients.length; i++) {
            for (uint256 j = 0; j < patients[i].centerIds.length; j++) {
                if (
                    keccak256(abi.encodePacked(patients[i].centerIds[j])) ==
                    keccak256(abi.encodePacked(_centerId))
                ) {
                    count++;
                    break;
                }
            }
        }
        return count;
    }

    function getPatientsTestedByCenter(string memory _centerId)
        public
        view
        returns (string[] memory, string[] memory)
    {
        uint256 count = getCountOfPatientsTestedByCenter(_centerId);

        string[] memory _patientIds = new string[](count);
        string[] memory testReportsOfPatients = new string[](count);
        uint256 index = 0;

        for (uint256 i = 0; i < patients.length; i++) {
            for (uint256 j = 0; j < patients[i].centerIds.length; j++) {
                if (
                    keccak256(abi.encodePacked(patients[i].centerIds[j])) ==
                    keccak256(abi.encodePacked(_centerId))
                ) {
                    _patientIds[index] = patients[i].id;
                    testReportsOfPatients[index] = patients[i].testReports[j];
                    index++;
                }
            }
        }
        return (_patientIds, testReportsOfPatients);
    }

    function getPatientReportsCount(string memory _patientId)
        public
        view
        returns (uint256)
    {
        require(isPatientRegistered(_patientId), "Patient ID does not exist.");
        uint256 patientIndex = getPatientIdIndex(_patientId);
        Patient storage patient = patients[patientIndex];
        return patient.testReports.length;
    }
}
