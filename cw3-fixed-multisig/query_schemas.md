# CW3 Fixed Multisig Query Messages


threshold - "Return ThresholdResponse"
{
    "threshold" : {

    }
}


proposal - Returns ProposalResponse
{
    "proposal": {
        proposal_id: 1
    }
}


list_proposals - Returns ProposalListResponse
{
    "list_proposals": {
        "limit": 1,
        "start_after": 1,


    }
}


reverse_proposals - Returns ProposalListResponse
{
    "reverse_proposals": {
        "limit": 1,
        "start_before": 1
    }
}



vote - Returns VoteResponse
{
    "vote": {
        "proposal_id": 1,
        "voter": "juno1rxjp2u9kxf7aqssxzcguuhvemwxlj0rj5tkgyh"
    }
}



list_votes - Returns VoteListResponse
{
    "list_votes": {
        "proposal_id": 1
    }
}


list_votes - Returns VoteListResponse (With Optional Parameters)
{
    "list_votes": {
        "proposal_id": 1,
        "limit": 1,
        "start_after": string|null???
    }
}



voter - Returns VoterInfo
{
    "voter": {
        "address": "juno1rxjp2u9kxf7aqssxzcguuhvemwxlj0rj5tkgyh"
    }
}




list_voters - Returns VoterListResponse
{
    "list_voters": {}
}


list_voters - Returns VoterListResponse (with optional parameters)
{
    "list_voters": {
        "limit":1,
        "start_after": string|null????
    },
}






